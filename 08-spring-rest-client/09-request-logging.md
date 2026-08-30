# How do you implement request/response logging? 🟡

## Question

How do you implement request and response logging in Spring REST clients?

## Answer

Request/response logging can be implemented using WebClient filters or by using logging interceptors. Logging helps with debugging and monitoring by capturing request details, response status, and timing information. Logging should be done carefully to avoid logging sensitive information.

Implementation approaches include using ExchangeFilterFunction for WebClient, ClientHttpRequestInterceptor for RestTemplate, and custom logging filters. Sensitive information like passwords should be masked or excluded from logs.

## Code Example

```java
@Component
public class LoggingFilter implements ExchangeFilterFunction {
    @Override
    public Mono<ClientResponse> filter(ClientRequest request, ExchangeFunction next) {
        long startTime = System.currentTimeMillis();
        return next.exchange(request).doOnSuccess(response -> {
            long duration = System.currentTimeMillis() - startTime;
            log.info("Request: {} {} - Status: {} - Duration: {}ms",
                    request.method(), request.url(),
                    response.statusCode(), duration);
        });
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of logging implementation and your ability to capture useful debugging information.

## ⚠️ Common Mistake
Logging sensitive information like passwords or tokens, creating security vulnerabilities.

## ⚡ Follow-up Questions

1. How do you mask sensitive information in logs?
2. What information should be logged for debugging?
3. How do you implement different log levels for different environments?

## 📚 Related Topics

- [WebClient Configuration](./03-webclient-configuration.md)
- [Error Handling](./04-error-handling.md)
- [02. Microservices Architecture](../02-microservices-architecture/13-monitoring-observability.md)

## Navigation

[← Previous Question](./08-json-serialization.md) | [Next Question →](./10-webclient-oauth2.md)
