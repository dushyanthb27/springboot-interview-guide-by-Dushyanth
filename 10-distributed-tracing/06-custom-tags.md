# How do you add custom tags to spans? 🟡

## Question

How do you add custom tags to spans in Spring Cloud Sleuth for additional context?

## Answer

Custom tags can be added to spans using the NewSpan annotation or by manually accessing the current span through Tracer. Tags provide additional context for spans, such as business metrics, user IDs, or operation types. Custom tags help in filtering and analyzing traces in Zipkin.

Implementation approaches include using @NewSpan annotation with tags, accessing the current span through Tracer, and adding tags programmatically. Tags should be used for indexed data that you want to search or filter by.

## Code Example

```java
@Service
public class OrderService {
    private final Tracer tracer;

    public Order createOrder(Order order) {
        Span span = tracer.currentSpan();
        span.tag("order.id", order.getId().toString());
        span.tag("order.type", order.getType().toString());
        span.tag("user.id", order.getUserId().toString());

        // Business logic
        return processOrder(order);
    }

    @NewSpan("process-payment")
    public void processPayment(Order order) {
        // Tags can be added via annotation
        // @NewSpan(name = "process-payment", tags = {"order-type", "payment-method"})
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of span customization and your ability to add business context to traces.

## ⚠️ Common Mistake
Adding too many tags to spans, which can impact performance and storage.

## ⚡ Follow-up Questions

1. What is the difference between tags and logs in spans?
2. How do you add logs to spans?
3. What are the best practices for tag naming?

## 📚 Related Topics

- [Spans and Traces](./04-spans-and-traces.md)
- [Asynchronous Tracing](./07-asynchronous-tracing.md)
- [Tracing Analysis](./09-tracing-analysis.md)

## Navigation

[← Previous Question](./05-trace-context-propagation.md) | [Next Question →](./07-asynchronous-tracing.md)
