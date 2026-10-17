# What is distributed tracing and why is it important? 🟢

## Question

What is distributed tracing and why is it important in microservices architecture? Explain the key concepts of traces, spans, and trace IDs.

## Answer

Distributed tracing is a technique for tracking requests as they flow through multiple services in a distributed system. It provides visibility into the entire request lifecycle, from entry point to exit, across all services involved. Distributed tracing helps debug issues, identify performance bottlenecks, and understand service dependencies.

**Key concepts:**

1. **Trace**: A complete end-to-end journey of a request through the system. A trace represents the entire operation from the client's perspective.

2. **Span**: A single unit of work within a trace. Each service or operation creates a span to track its work. Spans have:
   - Start time and end time
   - Parent span (for nested operations)
   - Tags (key-value metadata)
   - Logs (timestamped events)
   - Span context (trace ID, span ID)

3. **Trace ID**: A unique identifier that spans all services involved in a request. Every span in a trace shares the same trace ID, allowing you to correlate logs and metrics across services.

4. **Span ID**: A unique identifier for each span within a trace. Parent-child relationships between spans form a tree structure representing the call hierarchy.

**Why distributed tracing is critical in microservices:**

1. **Debugging complex interactions**: When a request fails, distributed tracing shows exactly which service failed and why, reducing debugging time from hours to minutes.

2. **Identifying latency issues**: Tracing shows which service or operation is slow, allowing you to optimize performance bottlenecks.

3. **Understanding service dependencies**: Tracing visualizes which services call which, helping you understand system architecture and identify unexpected dependencies.

4. **Monitoring system health**: Trace metrics (error rates, latency) provide system-wide health monitoring.

5. **Root cause analysis**: When an issue occurs, tracing helps identify the root cause by showing the complete request path.

Without distributed tracing, it's difficult to understand what happens when a request fails or is slow in a distributed system. Logs are scattered across services, and correlating them is nearly impossible without a trace ID.

## Code Example

```java
// Using Spring Cloud Sleuth for distributed tracing
@SpringBootApplication
public class OrderServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(OrderServiceApplication.class, args);
    }
}

@RestController
@RequestMapping("/api/orders")
public class OrderController {

    private final OrderService orderService;

    // Sleuth automatically creates a span for this HTTP request
    @GetMapping("/{id}")
    public Order getOrder(@PathVariable Long id) {
        return orderService.findById(id);
    }
}

@Service
public class OrderService {

    private final UserServiceClient userServiceClient;
    private final WebClient webClient;

    // Sleuth propagates trace context to downstream services
    public Order findById(Long id) {
        Order order = orderRepository.findById(id).orElseThrow();

        // This call includes trace context headers (X-B3-TraceId, X-B3-SpanId)
        User user = webClient.get()
            .uri("http://user-service/api/users/" + order.getUserId())
            .retrieve()
            .bodyToMono(User.class)
            .block();

        order.setUser(user);
        return order;
    }
}

// Custom span for complex operations
@Service
public class OrderService {

    @NewSpan("process-order")
    public Order processOrder(Order order) {
        // This creates a custom span with the name "process-order"
        // You can add tags and logs to the span
        return orderRepository.save(order);
    }

    @NewSpan("calculate-total")
    @Tag(key = "order.id", expression = "#order.id")
    public BigDecimal calculateTotal(Order order) {
        // Tag the span with order ID for filtering
        return order.getItems().stream()
            .map(Item::getPrice)
            .reduce(BigDecimal.ZERO, BigDecimal::add);
    }
}

// Manually creating spans for complex workflows
@Service
public class OrderService {

    private final Tracer tracer;

    public Order processOrder(Order order) {
        Span span = tracer.nextSpan()
            .name("process-order")
            .tag("order.id", order.getId().toString())
            .start();

        try (Tracer.SpanInScope ws = tracer.withSpanInScope(span)) {
            // Business logic here
            span.event("order-validated");
            return orderRepository.save(order);
        } finally {
            span.end();
        }
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of distributed tracing fundamentals and its importance in microservices. They're looking for knowledge of traces, spans, and trace IDs, and practical experience with tracing tools. A strong answer demonstrates understanding of how tracing differs from logging and how it enables debugging in distributed systems.

## ⚠️ Common Mistake

Confusing distributed tracing with logging. Logging captures events at a point in time within a single service, while tracing captures the end-to-end journey of a request across services. Another mistake is not instrumenting code properly—tracing only works if you create spans for meaningful operations and add tags/metadata to make traces searchable.

## ⚡ Follow-up Questions

1. What is the difference between distributed tracing and logging?
2. How does distributed tracing help with debugging in production?
3. What are the key components of a distributed tracing system (collector, storage, UI)?
4. How do you handle trace sampling to avoid overwhelming the tracing system?
5. What is the difference between OpenTelemetry, Zipkin, and Jaeger?

## 📚 Related Topics

- [How Sleuth Works](./02-how-sleuth-works.md)
- [Spans and Traces](./04-spans-and-traces.md)
- [02. Microservices Architecture](../02-microservices-architecture/README.md)

## Navigation

[← Main Section](./README.md) | [Next Question →](./02-how-sleuth-works.md)
