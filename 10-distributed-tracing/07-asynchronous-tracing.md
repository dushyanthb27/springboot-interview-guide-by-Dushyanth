# How do you implement asynchronous tracing? 🟠

## Question

How do you implement distributed tracing in asynchronous operations?

## Question

Asynchronous tracing requires special handling to ensure trace context is propagated across thread boundaries. Spring Cloud Sleuth provides support for tracing in asynchronous operations using ExecutorService wrappers, Reactor operators, and CompletableFuture integration. The trace context must be explicitly propagated when work is executed on different threads.

Implementation approaches include using Sleuth's ExecutorService wrappers, Reactor's tracing support, and manual context propagation using Tracer. Asynchronous operations must ensure the trace context is available in the new thread.

## Code Example

```java
@Configuration
public class AsyncConfig {
    @Bean
    public ExecutorService executorService(BeanFactory beanFactory) {
        return new TraceableExecutorService(Executors.newFixedThreadPool(10), beanFactory);
    }
}

@Service
public class OrderService {
    private final ExecutorService executorService;
    private final Tracer tracer;

    public CompletableFuture<Order> createOrderAsync(Order order) {
        Span span = tracer.nextSpan().name("create-order-async");
        try (Tracer.SpanInScope ws = tracer.withSpan(span.start())) {
            return CompletableFuture.supplyAsync(() -> processOrder(order), executorService);
        } finally {
            span.end();
        }
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of trace context propagation in asynchronous operations and your ability to handle thread boundaries.

## ⚠️ Common Mistake
Not propagating trace context across thread boundaries, leading to broken traces in asynchronous operations.

## ⚡ Follow-up Questions

1. How does Sleuth handle Reactor tracing?
2. What is TraceableExecutorService?
3. How do you manually propagate trace context?

## 📚 Related Topics

- [Trace Context Propagation](./05-trace-context-propagation.md)
- [Custom Tags](./06-custom-tags.md)
- [Message Tracing](./11-message-tracing.md)

## Navigation

[← Previous Question](./06-custom-tags.md) | [Next Question →](./08-opentelemetry-integration.md)
