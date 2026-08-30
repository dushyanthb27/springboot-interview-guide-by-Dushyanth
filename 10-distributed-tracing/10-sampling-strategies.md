# What are the sampling strategies? 🟡

## Question

What are the different sampling strategies in distributed tracing and when to use each?

## Answer

Sampling strategies determine which traces are collected to balance observability with performance and storage costs. Common strategies include probability-based sampling (collect a percentage of traces), rate-based sampling (collect a fixed number of traces per time period), and rule-based sampling (collect traces based on specific criteria).

Probability sampling is the most common, typically set to 10-20% for production. Rate-based sampling is useful for high-traffic systems. Rule-based sampling can be used to always sample error traces or specific user requests.

## Code Example

```java
// application.yml
spring:
  sleuth:
    sampler:
      probability: 0.1  # 10% of traces
      # rate: 100  # 100 traces per second
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of sampling strategies and your ability to balance observability with performance.

## ⚠️ Common Mistake
Setting sampling too high in production, leading to excessive storage costs and performance impact.

## ⚡ Follow-up Questions

1. How do you determine the right sampling rate?
2. What is the difference between probability and rate-based sampling?
3. How do you implement rule-based sampling?

## 📚 Related Topics

- [Sleuth Zipkin Integration](./03-sleuth-zipkin-integration.md)
- [Tracing Analysis](./09-tracing-analysis.md)
- [Best Practices](./12-best-practices.md)

## Navigation

[← Previous Question](./09-tracing-analysis.md) | [Next Question →](./11-message-tracing.md)
