# How does Spring Cloud Sleuth work? 🟡

## Question

How does Spring Cloud Sleuth implement distributed tracing in Spring Boot applications?

## Answer

Spring Cloud Sleuth provides distributed tracing support by automatically adding trace and span IDs to log statements and propagating trace context across HTTP, messaging, and database calls. It intercepts outgoing and incoming requests to add tracing headers (X-B3-TraceId, X-B3-SpanId) and creates spans for each operation.

Sleuth integrates with various communication protocols including HTTP, RabbitMQ, Kafka, and database calls. It automatically instruments common Spring components like RestTemplate, WebClient, and messaging templates, requiring minimal configuration.

## Code Example

```java
// Dependency
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-sleuth</artifactId>
</dependency>

// Automatic tracing - no code changes needed
@Service
public class UserService {
    private final RestTemplate restTemplate;

    public User getUser(Long userId) {
        // Sleuth automatically adds trace context to this call
        return restTemplate.getForObject("http://user-service/api/users/" + userId, User.class);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Spring Cloud Sleuth's automatic instrumentation and trace context propagation.

## ⚠️ Common Mistake
Not understanding that Sleuth automatically instruments common components, leading to manual implementation when not needed.

## ⚡ Follow-up Questions

1. What tracing headers does Sleuth use?
2. How does Sleuth integrate with different protocols?
3. What is the difference between Sleuth and OpenTelemetry?

## 📚 Related Topics

- [Sleuth Zipkin Integration](./03-sleuth-zipkin-integration.md)
- [Trace Context Propagation](./05-trace-context-propagation.md)
- [Spans and Traces](./04-spans-and-traces.md)

## Navigation

[← Previous Question](./01-what-is-distributed-tracing.md) | [Next Question →](./03-sleuth-zipkin-integration.md)
