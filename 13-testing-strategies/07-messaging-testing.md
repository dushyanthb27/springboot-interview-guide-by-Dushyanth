# How do you test message-driven applications? 🟠

## Question

How do you test message-driven applications with RabbitMQ or Kafka?

## Answer

Message-driven applications are tested using embedded message brokers or Testcontainers for real infrastructure. Spring provides @EmbeddedKafka for Kafka testing and @RabbitMQTest for RabbitMQ testing. These allow testing message production and consumption without external dependencies.

Implementation involves using embedded brokers or Testcontainers, sending test messages, and verifying message consumption and processing logic.

## Code Example

```java
@SpringBootTest
@EmbeddedKafka(partitions = 1, topics = "order-topic")
class OrderProducerTest {
    @Autowired
    private KafkaTemplate<String, Order> kafkaTemplate;

    @Autowired
    private KafkaListenerEndpointRegistry kafkaListenerEndpointRegistry;

    @Test
    void testSendOrder() {
        Order order = new Order(1L, 100);
        kafkaTemplate.send("order-topic", order);

        // Verify message was sent
        // Or verify consumer received message
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of message-driven testing and your ability to test messaging components.

## ⚠️ Common Mistake
Not properly configuring embedded brokers, leading to test failures.

## ⚡ Follow-up Questions

1. How do you test message consumers?
2. What is the difference between @EmbeddedKafka and Testcontainers?
3. How do you verify message processing?

## 📚 Related Topics

- [Integration Testing](./03-integration-testing.md)
- [Testcontainers](./06-testcontainers.md)
- [11. Message-Driven Microservices](../11-message-driven-microservices/README.md)

## Navigation

[← Previous Question](./06-testcontainers.md) | [Next Question →](./08-test-slices.md)
