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

