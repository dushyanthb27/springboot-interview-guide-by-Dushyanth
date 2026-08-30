# How do you integrate with OpenTelemetry? 🟠

## Question

How do you integrate Spring Boot applications with OpenTelemetry for distributed tracing?

## Answer

OpenTelemetry is a vendor-neutral observability framework that provides distributed tracing, metrics, and logging. Spring Boot can integrate with OpenTelemetry using the spring-boot-starter-opentelemetry dependency. OpenTelemetry provides a standard API for instrumentation and can export traces to various backends including Zipkin, Jaeger, and cloud providers.

Configuration includes adding the OpenTelemetry starter, configuring the exporter (OTLP, Zipkin, Jaeger), and setting up resource attributes for service identification. OpenTelemetry is becoming the standard for observability in cloud-native applications.

## Code Example

```java
// Dependency
<dependency>
    <groupId>io.opentelemetry</groupId>
    <artifactId>opentelemetry-spring-boot-starter</artifactId>
</dependency>

// application.yml
management:
  otlp:
    tracing:
      endpoint: http://localhost:4318
  tracing:
    sampling:
      probability: 1.0

spring:
  application:
    name: order-service
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of OpenTelemetry and your ability to integrate it with Spring Boot.

## ⚠️ Common Mistake
Not configuring the correct exporter endpoint, leading to trace data not being sent.

## ⚡ Follow-up Questions

1. What is the difference between OpenTelemetry and Sleuth?
2. What exporters does OpenTelemetry support?
3. How do you configure resource attributes?

## 📚 Related Topics

- [How Sleuth Works](./02-how-sleuth-works.md)
- [Sleuth Zipkin Integration](./03-sleuth-zipkin-integration.md)
- [Best Practices](./12-best-practices.md)

## Navigation

[← Previous Question](./07-asynchronous-tracing.md) | [Next Question →](./09-tracing-analysis.md)
