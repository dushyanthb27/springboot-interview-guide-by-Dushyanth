# How do you monitor and observe microservices? 🟡

## Question

What are the key aspects of monitoring and observability in microservices architecture and how do you implement them?

## Answer

Monitoring and observability in microservices consist of three pillars: logs (structured logging for debugging and auditing), metrics (quantitative data about system behavior like request rates, error rates, latency), and traces (distributed tracing to follow requests across services). Implementing these requires centralized logging (ELK stack, Loki), metrics collection (Prometheus, Grafana), and distributed tracing (Zipkin, Jaeger, OpenTelemetry).

Spring Boot Actuator provides production-ready endpoints for health checks, metrics, and tracing. For comprehensive observability, you need structured logging with correlation IDs, application metrics (business and technical), distributed tracing with span context propagation, and alerting based on metrics and logs.

## Code Example

```java
// Spring Boot Actuator Configuration
@Configuration
public class ActuatorConfig {

    @Bean
    public MeterRegistryCustomizer<MeterRegistry> metricsCommonTags() {
        return registry -> registry.config().commonTags(
                "application", "order-service",
                "region", System.getenv().getOrDefault("REGION", "unknown")
        );
    }
}

// Custom Metrics
@Service
public class OrderService {
    private final MeterRegistry meterRegistry;
    private final Counter orderCreatedCounter;
    private final Timer orderProcessingTimer;

    public OrderService(MeterRegistry meterRegistry) {
        this.meterRegistry = meterRegistry;
        this.orderCreatedCounter = Counter.builder("order.created")
                .description("Number of orders created")
                .register(meterRegistry);
        this.orderProcessingTimer = Timer.builder("order.processing")
                .description("Order processing time")
                .register(meterRegistry);
    }

    public Order createOrder(Order order) {
        return orderProcessingTimer.record(() -> {
            Order savedOrder = orderRepository.save(order);
            orderCreatedCounter.increment();
            return savedOrder;
        });
    }
}

// Structured Logging with Correlation ID
@Configuration
public class LoggingConfig {

    @Bean
    public Filter registrationBean() {
        return new OncePerRequestFilter() {
            @Override
            protected void doFilterInternal(HttpServletRequest request,
                                            HttpServletResponse response,
                                            FilterChain filterChain) throws ServletException, IOException {
                String traceId = request.getHeader("X-Trace-ID");
                if (traceId == null) {
                    traceId = UUID.randomUUID().toString();
                }
                MDC.put("traceId", traceId);
                try {
                    filterChain.doFilter(request, response);
                } finally {
                    MDC.remove("traceId");
                }
            }
        };
    }
}

// Distributed Tracing with Sleuth
@Service
public class OrderService {
    @NewSpan("process-order")
    public Order processOrder(Order order) {
        // This span will be part of the distributed trace
        User user = userServiceClient.getUser(order.getUserId());
        paymentService.processPayment(order.getPayment());
        return order;
    }
}

// Custom Health Indicator
@Component
public class DatabaseHealthIndicator implements HealthIndicator {
    @Autowired
    private OrderRepository orderRepository;

    @Override
    public Health health() {
        try {
            orderRepository.count();
            return Health.up().withDetail("database", "PostgreSQL").build();
        } catch (Exception e) {
            return Health.down(e).withDetail("database", "PostgreSQL").build();
        }
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of observability principles and your familiarity with implementing monitoring in distributed systems. They're looking for awareness of the three pillars of observability and practical implementation approaches.

## ⚠️ Common Mistake
Focusing only on logs without implementing metrics and distributed tracing. Another mistake is not using structured logging and correlation IDs, making it impossible to trace requests across services.

## ⚡ Follow-up Questions

1. What is the difference between monitoring and observability?
2. How do you implement distributed tracing across microservices?
3. What are the key metrics to monitor for microservices?

## 📚 Related Topics

- [10. Distributed Tracing](../10-distributed-tracing/README.md)
- [Cross-Cutting Concerns](./09-cross-cutting-concerns.md)
- [Deployment Patterns](./08-deployment-patterns.md)

## Navigation

[← Previous Question](./12-anti-patterns.md) | [Next Question →](./14-saga-pattern.md)
