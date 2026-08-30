# What are the different gateway routing patterns? 🟡

## Question

Explain the different routing patterns used in API Gateway and when to use each pattern.

## Answer

Common API Gateway routing patterns include simple routing (direct path-based routing), service-to-service routing (gateway routes to specific services), aggregation routing (gateway calls multiple services and aggregates responses), and routing based on headers/parameters (conditional routing based on request characteristics).

Other patterns include version-based routing (route to different service versions), canary routing (route percentage of traffic to new versions), and blue-green routing (switch between production versions). The choice depends on deployment strategies and traffic management requirements.

## Code Example

```java
// Simple routing
.route("user-service", r -> r.path("/api/users/**").uri("lb://user-service"))

// Aggregation routing
.route("order-aggregation", r -> r.path("/api/orders/complete/**")
    .filters(f -> f.modifyResponseBody(String.class, String.class,
        (response, body) -> aggregateResponses(response, body)))
    .uri("lb://order-service"))

// Header-based routing
.route("v1-api", r -> r.header("X-API-Version", "v1").uri("lb://user-service-v1"))
.route("v2-api", r -> r.header("X-API-Version", "v2").uri("lb://user-service-v2"))
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of different routing patterns and your ability to choose the right pattern for specific use cases.

## ⚠️ Common Mistake
Using simple routing when aggregation would be more efficient, leading to multiple client calls instead of a single gateway call.

## ⚡ Follow-up Questions

1. How do you implement canary deployments using routing?
2. What is the difference between blue-green and canary routing?
3. How do you implement request aggregation?

## 📚 Related Topics

- [Routing Configuration](./03-routing-configuration.md)
- [API Versioning](./11-api-versioning.md)
- [02. Microservices Architecture](../02-microservices-architecture/README.md)

## Navigation

[← Previous Question](./08-load-balancing.md) | [Next Question →](./10-monitoring.md)
