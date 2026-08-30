# How do you implement message retry and dead letter queues? 🟠

## Question

How do you implement message retry mechanisms and dead letter queues for failed messages?

## Answer

Message retry and DLQ are implemented using Spring's retry support and broker-specific DLQ configuration. Retry attempts to reprocess failed messages with exponential backoff. DLQ stores messages that failed after all retry attempts for later inspection and reprocessing.

Configuration includes setting retry policies with max attempts and backoff intervals, configuring DLQ for failed messages, and implementing monitoring to track retry and DLQ metrics.

## Code Example

```java
@Configuration
public class RabbitMQConfig {
    @Bean
    public Queue orderQueue() {
        return QueueBuilder.durable("order.queue")
                .withArgument("x-dead-letter-exchange", "order.dlx")
                .withArgument("x-dead-letter-routing-key", "order.dlq")
                .build();
    }

    @Bean
    public DirectExchange deadLetterExchange() {
        return new DirectExchange("order.dlx");
    }

    @Bean
    public Queue deadLetterQueue() {
        return new Queue("order.dlq");
    }

    @Bean
    public Binding deadLetterBinding() {
        return BindingBuilder.bind(deadLetterQueue()).to(deadLetterExchange()).with("order.dlq");
    }
}

@Service
public class OrderConsumer {
    @RabbitListener(queues = "order.queue")
    @Retryable(value = {DataAccessException.class}, maxAttempts = 3, backoff = @Backoff(delay = 1000, multiplier = 2))
    public void handleOrder(Order order) {
        // Process order
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of retry mechanisms and DLQ implementation for reliable message processing.

## ⚠️ Common Mistake
Not setting proper retry policies, leading to immediate message failure or excessive retry attempts.

## ⚡ Follow-up Questions

1. How do you configure exponential backoff?
2. What happens to messages in DLQ?
3. How do you reprocess messages from DLQ?

## 📚 Related Topics

- [Error Handling](./07-error-handling.md)
- [Idempotency](./09-idempotency.md)
- [07. Resilience Patterns](../07-resilience-patterns/README.md)

## Navigation

[← Previous Question](./07-error-handling.md) | [Next Question →](./09-idempotency.md)
