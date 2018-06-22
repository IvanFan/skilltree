```java
public class Limiter implements RateLimit {
    long time = -1;

    int qps;
    long qps_millis;

    @Override
    public boolean allowThisRequest() {
        long currentTime = System.currentTimeMillis();
        if (time == -1) {
            time = currentTime;
            return true;
        } else {
            long result = currentTime - time;
            if (result < qps_millis) {
                return false;
            } else {
                time = currentTime;
                return true;
            }
        }
    }

    @Override
    public void setQPS(int qps) {
        if (qps < 1 || qps > 1000000) {
            throw new RuntimeException();
        }

        this.qps = qps;
        qps_millis = qps * 1000;
    }
}
```

#### Leaky Bucket

[Leaky bucket](https://en.wikipedia.org/wiki/Leaky_bucket)\(closely related to[token bucket](https://en.wikipedia.org/wiki/Token_bucket)\) is an algorithm that provides a simple, intuitive approach to rate limiting via a queue which you can think of as a bucket holding the requests. When a request is registered, it is appended to the end of the queue. At a regular interval, the first item on the queue is processed. This is also known as a first in first out \(**FIFO**\) queue. If the queue is full, then additional requests are discarded \(or leaked\).

![](https://2tjosk2rxzc21medji3nfn1g-wpengine.netdna-ssl.com/wp-content/uploads/2017/12/02-rate-limit-kong.png)

The advantage of this algorithm is that it smooths out bursts of requests and processes them at an approximately average rate. It’s also easy to implement on a single server or load balancer, and is memory efficient for each user given the limited queue size.

However, a burst of traffic can fill up the queue with old requests and starve more recent requests from being processed. It also provides no guarantee that requests get processed in a fixed amount of time. Additionally, if you load balance servers for fault tolerance or increased throughput, you must use a policy to coordinate and enforce the limit between them. We will come back to challenges of distributed environments later.

#### Sliding Log

**Sliding Log**rate limiting involves tracking a time stamped log for each consumer’s request. These logs are usually stored in a hash set or table that is sorted by time. Logs with timestamps beyond a threshold are discarded. When a new request comes in, we calculate the sum of logs to determine the request rate. If the request would exceed the threshold rate, then it is held.

![](https://2tjosk2rxzc21medji3nfn1g-wpengine.netdna-ssl.com/wp-content/uploads/2017/12/04-rate-limit-kong.png)

The advantage of this algorithm is that it does not suffer from the boundary conditions of fixed windows. The rate limit will be enforced precisely. Also, because the sliding log is tracked for each consumer, you don’t have the stampede effect that challenges fixed windows. However, it can be very expensive to store an unlimited number of logs for every request. It’s also expensive to compute because each request requires calculating a summation over the consumer’s prior requests, potentially across a cluster of servers. As a result, it does not scale well to handle large bursts of traffic or denial of service attacks.

#### Sliding Window

This is a hybrid approach that combines the low processing cost of the fixed window algorithm, and the improved boundary conditions of the sliding log. Like the fixed window algorithm, we track a counter for each fixed window. Next, we account for a weighted value of the previous window’s request rate based on the current timestamp to smooth out bursts of traffic. For example, if the current window is 25% through, then we weight the previous window’s count by 75%. The relatively small number of data points needed to track per key allows us to scale and distribute across large clusters.

![](https://2tjosk2rxzc21medji3nfn1g-wpengine.netdna-ssl.com/wp-content/uploads/2017/12/05-rate-limit-kong.png)

We recommend the**sliding window**approach because it gives the flexibility to scale rate limiting with good performance. The rate windows are an intuitive way she to present rate limit data to API consumers. It also avoids the starvation problem of leaky bucket, and the bursting problems of fixed window implementations.



#### Race Conditions

One of the largest problems with a centralized data store is the potential for[race conditions](https://en.wikipedia.org/wiki/Race_condition)in[high concurrency](https://en.wikipedia.org/wiki/Concurrency_%28computer_science%29)request patterns. This happens when you use a naïve “get-then-set” approach, wherein you retrieve the current rate limit counter, increment it, and then push it back to the datastore. The problem with this model is that in the time it takes to perform a full cycle of read-increment-store, additional requests can come through, each attempting to store the increment counter with an invalid \(lower\) counter value. This allows a consumer sending a very high rate of requests to bypass rate limiting controls.



![](https://2tjosk2rxzc21medji3nfn1g-wpengine.netdna-ssl.com/wp-content/uploads/2017/12/06-2-rate-limit-kong.png)



One way to avoid this problem is to put a “**lock**” around the key in question, preventing any other processes from accessing or writing to the counter. This would quickly become a major performance bottleneck, and does not scale well, particularly when using remote servers like Redis as the backing datastore.

A better approach is to use a “**set-then-get**” mindset, relying on atomic operators that implement locks in a very performant fashion, allowing you to quickly increment and check counter values without letting the atomic operations get in the way.

### The algorithm {#9431}

Each bucket has a few properties.

* **key**
  : a unique byte string that identifies the bucket
* **maxAmount**
  : the maximum number of tokens the bucket can hold
* **refillTime**
  : the amount of time between refills
* **refillAmount**
  : the number of tokens that are added to the bucket during a refill
* **value**
  : the current number of tokens in the bucket
* **lastUpdate**
  : the last time the bucket was updated

There are two operations.**get\(\)**simply returns the current value.**get\(\)**isn’t actually used very often at Smyte, since we generally want to decrement the rate limiter and return its current value in one, low-latency operation. This operation is called**reduce\(tokens\)**.

First, if the bucket doesn’t exist, we create it:

```
bucket.value = bucket.maxAmount


bucket.lastUpdate = now()
```

Then we refill the bucket if there are any pending refills:

```
refillCount = floor(
(now() - bucket.lastUpdate) / bucket.refillTime)
```

```
bucket.value = min(


    bucket.maxAmount,


    bucket.value + refillCount * bucket.refillAmount


)
```

```
bucket.lastUpdate = min(


    now(), 


    bucket.lastUpdate + refillCount * bucket.refillTime


)
```

Next, we check if`tokens >= bucket.value`and bail out if it is true.

Finally, we take the tokens out of the bucket:

```
bucket.value -= tokens
```

You can see a simple Python implementation in[this gist](https://gist.github.com/petehunt/08c9fef5703e79ca26ceed845163dcdc). The example shows it’s very easy to implement this algorithm in memory, and can be quite cheap since we’re only storing two integers for each bucket. One problem with implementing this in memory is persistence. We may have very long-lived rate limits in our application, and we can’t afford to lose them if we take down or move a rate limit shard.

Given that the data fits in memory and that low latency and persistence are hard requirements, Redis seems like the natural choice for this application.

  


