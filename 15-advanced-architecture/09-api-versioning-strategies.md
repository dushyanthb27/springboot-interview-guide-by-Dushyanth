# How do you implement API versioning strategies? 🟡

## Question

What are the different API versioning strategies and when to use each?

## Answer

API versioning strategies include URL-based (/v1/api), header-based (Accept: application/vnd.v1+json), query parameter-based (?version=1), and content negotiation. Each strategy has trade-offs in terms of URL cleanliness, caching, and implementation complexity.

URL-based versioning is most common and cache-friendly. Header-based is cleaner but harder to use. Query parameter-based is simple but can conflict with other parameters. The choice depends on API requirements and client expectations.

## Code Example

```java
@RestController
@RequestMapping("/api/v1/orders")
public class OrderControllerV1 {
    // V1 implementation
}

@RestController
@RequestMapping("/api/v2/orders")
public class OrderControllerV2 {
    // V2 implementation with breaking changes
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of API versioning strategies and your ability to choose the right approach.

## ⚠️ Common Mistake
Not planning for versioning from the beginning, making it difficult to introduce breaking changes later.

## ⚡ Follow-up Questions

1. What are the trade-offs of each versioning strategy?
2. How do you handle backward compatibility?
3. When should you introduce a new API version?

## 📚 Related Topics

- [05. API Gateway Patterns](../05-api-gateway-patterns/README.md)
- [03. Spring Data REST](../03-spring-data-rest/README.md)
- [08. Spring Rest Client](../08-spring-rest-client/README.md)

## Navigation

[← Previous Question](./08-distributed-caching.md) | [Next Question →](./10-feature-flags.md)
