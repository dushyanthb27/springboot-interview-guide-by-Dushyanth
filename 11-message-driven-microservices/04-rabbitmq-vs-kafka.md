# What are the differences between RabbitMQ and Kafka? 🟡

## Question

What are the key differences between RabbitMQ and Kafka and when would you choose each?

## Answer

RabbitMQ is a traditional message broker based on AMQP, designed for reliable message delivery with features like queues, exchanges, and bindings. Kafka is a distributed event streaming platform designed for high-throughput, low-latency event streaming with topics and partitions.

RabbitMQ is better for complex routing patterns, request-reply patterns, and when you need fine-grained message control. Kafka is better for event streaming, high-throughput scenarios, and when you need message replay capabilities. RabbitMQ stores messages until consumed, while Kafka retains messages for a configurable retention period.

## Code Example

```java
// RabbitMQ: Queue-based, fine-grained routing
@RabbitListener(queues = "order.queue")
public void handleOrder(Order order) {
    // Process order
}

// Kafka: Topic-based, event streaming
@KafkaListener(topics = "order-topic", groupId = "order-group")
public void handleOrder(Order order) {
    // Process order
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of the differences between message brokers and your ability to choose the right tool for the job.

## ⚠️ Common Mistake
Using Kafka for simple request-reply patterns where RabbitMQ would be more appropriate.

## ⚡ Follow-up Questions

1. When would you choose RabbitMQ over Kafka?
2. What are the throughput differences between RabbitMQ and Kafka?
3. How does message retention differ between the two?

## 📚 Related Topics

- [RabbitMQ Implementation](./02-rabbitmq-implementation.md)
- [Kafka Implementation](./03-kafka-implementation.md)
- [Message Patterns](./05-message-patterns.md)

## Navigation

[← Previous Question](./03-kafka-implementation.md) | [Next Question →](./05-message-patterns.md)
