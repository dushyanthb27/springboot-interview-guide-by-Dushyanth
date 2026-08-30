# What are the advanced architectural patterns? 🟢

## Question

What are the advanced architectural patterns used in microservices beyond the basics?

## Answer

Advanced architectural patterns include Strangler Fig (gradual migration), Sidecar (cross-cutting concerns), Ambassador (service mesh), Anti-Corruption Layer (legacy integration), Bulkhead (resource isolation), Circuit Breaker (fault tolerance), and Saga (distributed transactions). These patterns solve complex challenges in distributed systems.

These patterns address specific challenges like legacy system integration, cross-cutting concerns, fault tolerance, and distributed data management. Understanding these patterns is essential for building robust microservices at scale.

## Code Example

```java
// Sidecar pattern concept
// Main application container + Sidecar container
// Sidecar handles logging, monitoring, security
// Main application focuses on business logic
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of advanced patterns and your ability to apply them appropriately.

## ⚡ Follow-up Questions

1. When would you use the Strangler Fig pattern?
2. What is the difference between Sidecar and Ambassador?
3. How do patterns solve distributed system challenges?

## 📚 Related Topics

- [Strangler Fig](./02-strangler-fig.md)
- [Sidecar Pattern](./03-sidecar-pattern.md)
- [02. Microservices Architecture](../02-microservices-architecture/README.md)

## Navigation

[← Main Section](./README.md) | [Next Question →](./02-strangler-fig.md)
