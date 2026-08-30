# How do you integrate Sleuth with Zipkin? 🟡

## Question

How do you integrate Spring Cloud Sleuth with Zipkin for distributed tracing visualization?

## Answer

Zipkin integration with Spring Cloud Sleuth is done by adding the spring-cloud-sleuth-zipkin dependency and configuring the Zipkin server URL. Sleuth automatically sends trace data to Zipkin, which provides a web UI for visualizing traces and analyzing request flow across services.

Configuration includes setting the Zipkin server URL, configuring the sender type (HTTP or RabbitMQ/Kafka), and optionally configuring sampling rates to control the amount of trace data collected.

## Code Example

```java
// Dependency
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-sleuth-zipkin</artifactId>
</dependency>

// application.yml
spring:
  sleuth:
    zipkin:
      base-url: http://localhost:9411
      sender:
        type: web
  zipkin:
    sender:
      type: web
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Zipkin integration and your ability to set up distributed tracing visualization.

## ⚠️ Common Mistake
Not configuring the correct Zipkin server URL, leading to trace data not being sent.

## ⚡ Follow-up Questions

1. How do you run Zipkin locally?
2. What is the difference between HTTP and message-based senders?
3. How do you configure sampling rates?

## 📚 Related Topics

- [How Sleuth Works](./02-how-sleuth-works.md)
- [Tracing Analysis](./09-tracing-analysis.md)
- [Sampling Strategies](./10-sampling-strategies.md)

## Navigation

[← Previous Question](./02-how-sleuth-works.md) | [Next Question →](./04-spans-and-traces.md)
