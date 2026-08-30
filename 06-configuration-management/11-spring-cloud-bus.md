# How do you integrate Config Server with Spring Cloud Bus? 🟠

## Question

How does Spring Cloud Bus enable automatic configuration refresh across multiple instances?

## Answer

Spring Cloud Bus links multiple instances of a Config Server and clients using a message broker like RabbitMQ or Kafka. When configuration changes are committed to the repository, a message is published to the bus, triggering refresh on all connected instances. This enables automatic configuration propagation without manual intervention.

Integration involves adding the spring-cloud-starter-bus-amqp or spring-cloud-starter-bus-kafka dependency, configuring the message broker connection, and using the /actuator/bus-refresh endpoint to trigger refresh across all instances.

## Code Example

```java
// Dependencies
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bus-amqp</artifactId>
</dependency>

// Configuration
spring:
  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest

management:
  endpoints:
    web:
      exposure:
        include: bus-refresh

// Trigger refresh across all instances
// POST http://localhost:8888/actuator/bus-refresh
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of distributed configuration updates and your ability to implement automatic refresh across multiple instances.

## ⚠️ Common Mistake
Not properly configuring the message broker connection, leading to refresh failures.

## ⚡ Follow-up Questions

1. How does Spring Cloud Bus differ from manual refresh?
2. What message brokers are supported by Spring Cloud Bus?
3. How do you handle refresh failures in a distributed environment?

## 📚 Related Topics

- [Configuration Refresh](./05-configuration-refresh.md)
- [Environment Config](./06-environment-config.md)
- [11. Message-Driven Microservices](../11-message-driven-microservices/README.md)

## Navigation

[← Previous Question](./10-best-practices.md) | [Next Question →](./12-alternatives.md)
