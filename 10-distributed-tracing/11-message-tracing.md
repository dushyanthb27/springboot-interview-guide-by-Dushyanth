# How do you implement tracing in message-driven systems? 🟠

## Question

How do you implement distributed tracing in message-driven microservices using RabbitMQ or Kafka?

## Answer

Message-driven tracing requires propagating trace context through message headers. Spring Cloud Sleuth automatically instruments RabbitMQ and Kafka by adding trace headers to messages. The trace context is extracted from incoming messages and added to outgoing messages, maintaining the trace across message boundaries.

Configuration includes enabling Sleuth's messaging instrumentation, configuring the message broker, and ensuring trace context headers are not removed by message transformations. Sleuth handles both synchronous and asynchronous messaging patterns.

## Code Example

```java
// Dependency
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-sleuth</artifactId>
</dependency>

// Automatic tracing - no code changes needed
@Service
public class OrderService {
    @RabbitListener(queues = "order.queue")
    public void processOrder(Order order) {
        // Sleuth automatically extracts trace context from message
        // and creates a span for this message processing
        processOrderLogic(order);
    }

    public void sendOrderEvent(Order order) {
        // Sleuth automatically adds trace headers to message
        rabbitTemplate.convertAndSend("order.exchange", "order.key", order);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of tracing in message-driven systems and your ability to maintain trace context across message boundaries.

## ⚠️ Common Mistake
Removing trace headers during message transformation, breaking the trace continuity.

## ⚡ Follow-up Questions

1. How does Sleuth instrument RabbitMQ and Kafka?
2. What headers are used for message tracing?
3. How do you handle message batching with tracing?

## 📚 Related Topics

- [Trace Context Propagation](./05-trace-context-propagation.md)
- [Asynchronous Tracing](./07-asynchronous-tracing.md)
- [11. Message-Driven Microservices](../11-message-driven-microservices/README.md)

## Navigation

[← Previous Question](./10-sampling-strategies.md) | [Next Question →](./12-best-practices.md)
