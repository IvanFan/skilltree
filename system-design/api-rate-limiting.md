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



