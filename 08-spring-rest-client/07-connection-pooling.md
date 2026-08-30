# How do you implement connection pooling in WebClient? 🟠

## Question

How do you implement connection pooling in WebClient to optimize resource utilization?

## Answer

WebClient connection pooling is configured through the underlying HttpClient, which can be set up with connection pool settings like maximum connections, connection timeout, and idle timeout. Connection pooling improves performance by reusing connections and limiting the number of concurrent connections.

Configuration includes setting max connections, connection timeout, idle timeout, and pool size. Connection pooling is essential for high-throughput scenarios to prevent resource exhaustion and improve performance.

## Code Example

```java
@Configuration
public class WebClientConfig {
    @Bean
    public WebClient webClient() {
        ConnectionProvider provider = ConnectionProvider.builder()
                .maxConnections(500)
                .pendingAcquireMaxCount(100)
                .build();

        HttpClient httpClient = HttpClient.create(provider)
                .option(ChannelOption.CONNECT_TIMEOUT_MILLIS, 5000)
                .responseTimeout(Duration.ofSeconds(5));

        return WebClient.builder()
                .clientConnector(new ReactorClientHttpConnector(httpClient))
                .baseUrl("http://user-service")
                .build();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of connection pooling and your ability to optimize WebClient performance.

## ⚠️ Common Mistake
Not configuring connection pooling, leading to resource exhaustion under high load.

## ⚡ Follow-up Questions

1. How do you determine the optimal pool size?
2. What is the difference between max connections and pending acquire count?
3. How do you monitor connection pool metrics?

## 📚 Related Topics

- [WebClient Configuration](./03-webclient-configuration.md)
- [Monitoring Resilience](../07-resilience-patterns/08-monitoring-resilience.md)
- [02. Microservices Architecture](../02-microservices-architecture/README.md)

## Navigation

[← Previous Question](./06-resttemplate-discovery.md) | [Next Question →](./08-json-serialization.md)
