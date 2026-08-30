# How do you implement messaging with Kafka? 🟡

## Question

How do you implement message producers and consumers using Spring Boot with Kafka?

## Answer

Kafka integration with Spring Boot is done using spring-kafka. Producers send messages using KafkaTemplate, and consumers receive messages using @KafkaListener annotation. Configuration includes defining topics, configuring producer/consumer properties, and setting up serialization/deserialization.

Implementation involves creating topics, configuring KafkaTemplate for sending messages, and using @KafkaListener for consuming messages. Spring Kafka handles connection management, offset management, and message conversion automatically.

## Code Example

```java
@Configuration
public class KafkaConfig {
    @Bean
    public NewTopic orderTopic() {
        return TopicBuilder.name("order-topic")
                .partitions(3)
                .replicas(1)
                .build();
    }
}

@Service
public class OrderProducer {
    private final KafkaTemplate<String, Order> kafkaTemplate;

    public void sendOrder(Order order) {
        kafkaTemplate.send("order-topic", order.getId().toString(), order);
    }
}

@Service
public class OrderConsumer {
    @KafkaListener(topics = "order-topic", groupId = "order-group")
    public void handleOrder(Order order) {
        // Process order
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Kafka integration and your ability to implement message producers and consumers.

## ⚠️ Common Mistake
Not configuring proper consumer groups, leading to duplicate message processing.

## ⚡ Follow-up Questions

1. What is the difference between partitions and consumer groups?
2. How does Kafka handle message ordering?
3. How do you configure offset management?

## 📚 Related Topics

- [RabbitMQ vs Kafka](./04-rabbitmq-vs-kafka.md)
- [Message Patterns](./05-message-patterns.md)
- [Message Ordering](./11-message-ordering.md)

## Navigation

[← Previous Question](./02-rabbitmq-implementation.md) | [Next Question →](./04-rabbitmq-vs-kafka.md)
