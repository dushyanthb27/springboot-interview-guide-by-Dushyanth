# How do you handle message serialization? 🟡

## Question

How do you handle message serialization and deserialization in Spring messaging?

## Answer

Message serialization in Spring messaging is handled by message converters. Spring provides built-in converters for JSON, XML, and other formats. Serialization can be configured using Jackson for JSON or custom converters for specific requirements. Both RabbitMQ and Kafka support configurable message converters.

Configuration includes setting up Jackson ObjectMapper with desired settings, configuring message converters in RabbitTemplate/KafkaTemplate, and ensuring consistent serialization across producers and consumers.

## Code Example

```java
@Configuration
public class RabbitMQConfig {
    @Bean
    public Jackson2JsonMessageConverter converter() {
        ObjectMapper objectMapper = new ObjectMapper()
                .registerModule(new JavaTimeModule())
                .disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);

        return new Jackson2JsonMessageConverter(objectMapper);
    }

    @Bean
    public RabbitTemplate rabbitTemplate(ConnectionFactory connectionFactory) {
        RabbitTemplate template = new RabbitTemplate(connectionFactory);
        template.setMessageConverter(converter());
        return template;
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of message serialization and your ability to configure it properly.

## ⚠️ Common Mistake
Not configuring consistent serialization across producers and consumers, leading to deserialization errors.

## ⚡ Follow-up Questions

1. How do you handle date serialization?
2. What are the different message converter types?
3. How do you implement custom serialization?

## 📚 Related Topics

- [RabbitMQ Implementation](./02-rabbitmq-implementation.md)
- [Kafka Implementation](./03-kafka-implementation.md)
- [Error Handling](./07-error-handling.md)

## Navigation

[← Previous Question](./05-message-patterns.md) | [Next Question →](./07-error-handling.md)
