# How do you propagate trace context across services? 🟡

## Question

How does trace context propagate across different services in a distributed system?

## Answer

Trace context propagation is achieved through HTTP headers (X-B3-TraceId, X-B3-SpanId, X-B3-ParentSpanId) or message properties in messaging systems. When a service makes a call to another service, it includes the trace context in the request headers. The receiving service extracts the context and continues the trace by creating child spans.

Spring Cloud Sleuth automatically handles context propagation for HTTP, messaging, and database calls. The context includes the trace ID, span ID, and parent span ID to maintain the trace hierarchy across service boundaries.

## Code Example

```java
// Automatic context propagation - no code changes needed
@Service
public class OrderService {
    private final RestTemplate restTemplate;

    public Order createOrder(Order order) {
        // Sleuth automatically adds trace headers to this call
        User user = restTemplate.getForObject("http://user-service/api/users/" + order.getUserId(), User.class);
        // Sleuth automatically extracts trace context from response
        return processOrder(order, user);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of trace context propagation and how Sleuth handles it automatically.

## ⚠️ Common Mistake
Manually implementing trace context propagation when Sleuth handles it automatically, adding unnecessary complexity.

## ⚡ Follow-up Questions

1. What headers are used for trace context propagation?
2. How does context propagation work in messaging systems?
3. What happens if trace context is lost?

## 📚 Related Topics

- [How Sleuth Works](./02-how-sleuth-works.md)
- [Spans and Traces](./04-spans-and-traces.md)
- [Message Tracing](./11-message-tracing.md)

## Navigation

[← Previous Question](./04-spans-and-traces.md) | [Next Question →](./06-custom-tags.md)
