# How do you configure WebClient? 🟡

## Question

What are the different ways to configure WebClient in Spring Boot?

## Answer

WebClient can be configured using WebClient.Builder for custom configuration including base URL, default headers, connection pool settings, and message converters. Configuration can be done through @Bean methods or directly using WebClient.create(). Advanced configuration includes customizing the underlying HTTP client, setting timeouts, and configuring SSL.

Configuration approaches include simple configuration with base URL, customizing with connection pooling, adding default headers, and configuring message converters for JSON/XML processing.

## Code Example

```java
@Configuration
public class WebClientConfig {
    @Bean
    public WebClient webClient() {
        HttpClient httpClient = HttpClient.create()
                .option(ChannelOption.CONNECT_TIMEOUT_MILLIS, 5000)
                .responseTimeout(Duration.ofSeconds(5));

        return WebClient.builder()
                .baseUrl("http://user-service")
                .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
                .clientConnector(new ReactorClientHttpConnector(httpClient))
                .build();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of WebClient configuration and your ability to customize it for specific requirements.

## ⚠️ Common Mistake
Not configuring timeouts, leading to indefinite waiting for unresponsive services.

## ⚡ Follow-up Questions

1. How do you configure SSL for WebClient?
2. What are the different message converters available?
3. How do you configure connection pooling?

## 📚 Related Topics

- [Connection Pooling](./07-connection-pooling.md)
- [Error Handling](./04-error-handling.md)
- [Retry Logic](./05-retry-logic.md)

## Navigation

[← Previous Question](./02-webclient-vs-resttemplate.md) | [Next Question →](./04-error-handling.md)
