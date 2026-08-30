# How do you implement message patterns (pub/sub, point-to-point)? 🟡

## Question

How do you implement common messaging patterns like publish/subscribe and point-to-point?

## Answer

Publish/Subscribe pattern sends messages to multiple consumers using fanout or topic exchanges in RabbitMQ or topics in Kafka. Point-to-point pattern sends messages to a single consumer using queues in RabbitMQ or partitions in Kafka. These patterns enable different communication patterns for different use cases.

Implementation involves configuring exchanges/queues for the desired pattern, using appropriate routing keys for topic exchanges, and configuring consumer groups for point-to-point patterns.

## Code Example

```java
// Publish/Subscribe with RabbitMQ
@Configuration
public class PubSubConfig {
    @Bean
    public FanoutExchange orderExchange() {
        return new FanoutExchange("order.fanout");
    }

    @Bean
    public Queue notificationQueue() {
        return new Queue("notification.queue");
    }

    @Bean
    public Queue analyticsQueue() {
        return new Queue("analytics.queue");
    }

    @Bean
    public Binding notificationBinding() {
        return BindingBuilder.bind(notificationQueue()).to(orderExchange());
    }

    @Bean
    public Binding analyticsBinding() {
        return BindingBuilder.bind(analyticsQueue()).to(orderExchange());
    }
}

// Point-to-Point with Kafka
@KafkaListener(topics = "order-topic", groupId = "order-processor")
public void handleOrder(Order order) {
    // Only one consumer in the group processes each message
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of messaging patterns and your ability to implement them.

## ⚠️ Common Mistake
Not understanding the difference between pub/sub and point-to-point, leading to incorrect pattern implementation.

## ⚡ Follow-up Questions

1. When would you use pub/sub vs point-to-point?
2. How do you implement request-reply pattern?
3. What is the difference between fanout and topic exchanges?

## 📚 Related Topics

- [RabbitMQ Implementation](./02-rabbitmq-implementation.md)
- [Kafka Implementation](./03-kafka-implementation.md)
- [Consumer Scaling](./12-consumer-scaling.md)

## Navigation

[← Previous Question](./04-rabbitmq-vs-kafka.md) | [Next Question →](./06-message-serialization.md)
