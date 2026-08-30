# How do you implement contract testing? 🟠

## Question

How do you implement contract testing for microservices using Spring Cloud Contract?

## Answer

Contract testing ensures that producer and consumer services have compatible API contracts. Spring Cloud Contract provides DSL for defining contracts, generates tests for producers, and stubs for consumers. This prevents breaking changes and ensures API compatibility across services.

Implementation involves defining contracts in DSL, generating producer tests automatically, and using stubs for consumer testing. Contracts are stored in a shared repository for versioning.

## Code Example

```java
// Contract definition (Groovy DSL)
org.springframework.cloud.contract.spec.Contract.make {
    request {
        method 'GET'
        url '/api/orders/1'
        headers {
            header('Content-Type', 'application/json')
        }
    }
    response {
        status 200
        body([
            id: 1,
            amount: 100
        ])
        headers {
            header('Content-Type': 'application/json')
        }
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of contract testing and your ability to ensure API compatibility.

## ⚠️ Common Mistake
Not updating contracts when APIs change, leading to integration failures.

## ⚡ Follow-up Questions

1. What is the difference between consumer-driven and provider-driven contracts?
2. How do you generate stubs from contracts?
3. How do you handle contract versioning?

## 📚 Related Topics

- [REST API Testing](./04-rest-api-testing.md)
- [Microservices Testing](./11-microservices-testing.md)
- [02. Microservices Architecture](../02-microservices-architecture/README.md)

## Navigation

[← Previous Question](./09-security-testing.md) | [Next Question →](./11-microservices-testing.md)
