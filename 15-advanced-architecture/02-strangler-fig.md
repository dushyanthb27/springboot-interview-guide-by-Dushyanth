# How do you implement the Strangler Fig pattern? 🟠

## Question

How do you implement the Strangler Fig pattern for migrating from monolith to microservices?

## Answer

The Strangler Fig pattern gradually replaces a monolith with microservices by creating a facade that routes requests to either the monolith or new microservices. Over time, more functionality is moved to microservices until the monolith is completely replaced. This enables incremental migration without a big bang rewrite.

Implementation involves creating an API Gateway or facade that routes requests, implementing new functionality as microservices, and gradually migrating existing functionality from the monolith to microservices.

## Code Example

```java
@Configuration
public class StranglerFigConfig {
    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
        return builder.routes()
                // New microservice
                .route("order-service", r -> r
                        .path("/api/orders/**")
                        .uri("lb://order-service"))
                // Legacy monolith
                .route("legacy", r -> r
                        .path("/api/**")
                        .uri("lb://legacy-monolith"))
                .build();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of the Strangler Fig pattern and your ability to plan incremental migration.

## ⚠️ Common Mistake
Trying to migrate everything at once instead of incrementally, leading to high risk and complexity.

## ⚡ Follow-up Questions

1. How do you handle data migration during strangler fig?
2. What are the risks of this pattern?
3. How do you verify migration success?

## 📚 Related Topics

- [02. Microservices Architecture](../02-microservices-architecture/README.md)
- [05. API Gateway Patterns](../05-api-gateway-patterns/README.md)
- [Anti-Corruption Layer](./05-anti-corruption-layer.md)

## Navigation

[← Previous Question](./01-advanced-patterns.md) | [Next Question →](./03-sidecar-pattern.md)
