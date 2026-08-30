# How do you handle API versioning in Gateway? 🟠

## Question

What are the strategies for implementing API versioning through Spring Cloud Gateway?

## Answer

API versioning in Gateway can be implemented using URL-based versioning (/v1/api/users), header-based versioning (X-API-Version header), content negotiation (Accept header), or query parameter versioning. The gateway can route requests to different service versions based on version information, enabling blue-green deployments and canary releases.

Versioning strategies include maintaining multiple service versions, routing based on version identifiers, and implementing backward compatibility. The gateway can also aggregate responses from different versions or implement version-specific transformations.

## Code Example

```java
// URL-based versioning
.route("user-service-v1", r -> r.path("/v1/api/users/**").uri("lb://user-service-v1"))
.route("user-service-v2", r -> r.path("/v2/api/users/**").uri("lb://user-service-v2"))

// Header-based versioning
.route("user-service-v1", r -> r.header("X-API-Version", "v1").uri("lb://user-service-v1"))
.route("user-service-v2", r -> r.header("X-API-Version", "v2").uri("lb://user-service-v2"))

// Canary routing (10% traffic to v2)
.route("user-service-canary", r -> r
    .path("/api/users/**")
    .and()
    .weight("user-service-v1", 90)
    .and()
    .weight("user-service-v2", 10))
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of API versioning strategies and your ability to implement them through gateway routing.

## ⚠️ Common Mistake
Not planning for versioning from the beginning, making it difficult to introduce breaking changes later.

## ⚡ Follow-up Questions

1. What are the pros and cons of different versioning strategies?
2. How do you handle backward compatibility during version transitions?
3. How do you implement canary deployments using gateway routing?

## 📚 Related Topics

- [Routing Patterns](./09-routing-patterns.md)
- [Routing Configuration](./03-routing-configuration.md)
- [02. Microservices Architecture](../02-microservices-architecture/README.md)

## Navigation

[← Previous Question](./10-monitoring.md) | [Next Question →](./12-alternatives.md)
