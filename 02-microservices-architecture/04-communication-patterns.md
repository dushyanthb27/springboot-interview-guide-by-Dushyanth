# What are the different communication patterns in microservices? 🟡

## Question

Explain the various communication patterns used in microservices architecture and when to use each pattern.

## Answer

Microservices communication patterns include synchronous communication (REST/gRPC for request-response, suitable for immediate responses), asynchronous communication (message queues for decoupled communication, suitable for eventual consistency and high throughput), event-driven architecture (services communicate through events, suitable for loose coupling and scalability), and hybrid approaches (combining synchronous and asynchronous based on use case).

Synchronous patterns include direct REST calls, API gateway routing, and service mesh communication. Asynchronous patterns include message brokers (RabbitMQ, Kafka), event streaming, and publish-subscribe models. The choice depends on requirements like response time needs, consistency requirements, coupling preferences, and scalability needs.

## Code Example

```java
// Synchronous Communication with REST
@Service
public class OrderService {
    private final RestTemplate restTemplate;
    private final WebClient webClient;

    // Using RestTemplate
    public User getUserSync(Long userId) {
        return restTemplate.getForObject(
                "http://user-service/api/users/" + userId,
                User.class
        );
    }

    // Using WebClient (reactive)
    public Mono<User> getUserAsync(Long userId) {
        return webClient.get()
                .uri("http://user-service/api/users/" + userId)
                .retrieve()
                .bodyToMono(User.class);
    }
}

// Asynchronous Communication with Message Queue
@Service
public class OrderEventPublisher {
    private final RabbitTemplate rabbitTemplate;

    public void publishOrderCreated(Order order) {
        rabbitTemplate.convertAndSend("order.exchange", "order.created", order);
    }
}

@RabbitListener(queues = "order.created.queue")
public class InventoryService {
    @RabbitHandler
    public void handleOrderCreated(Order order) {
        // Process order asynchronously
        reserveItems(order.getItems());
    }
}

// Event-Driven Architecture
@Service
public class UserEventPublisher {
    private final ApplicationEventPublisher eventPublisher;

    public void publishUserCreated(User user) {
        eventPublisher.publishEvent(new UserCreatedEvent(user));
    }
}

@EventListener
public class NotificationService {
    public void handleUserCreated(UserCreatedEvent event) {
        sendWelcomeEmail(event.getUser());
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of communication patterns and your ability to choose the right pattern for different scenarios. They're looking for awareness of trade-offs between synchronous and asynchronous communication.

## ⚠️ Common Mistake

Using synchronous communication for everything, leading to tight coupling and performance issues. Another mistake is not understanding when to use asynchronous patterns for better scalability and resilience.

## ⚡ Follow-up Questions

1. How do you handle failures in synchronous service communication?
2. What are the advantages of using message queues over direct HTTP calls?
3. How do you choose between REST and gRPC for service communication?

## 📚 Related Topics

- [Event-Driven Architecture](./10-event-driven-architecture.md)
- [Data Consistency](./05-data-consistency.md)
- [08. Spring Rest Client](../08-spring-rest-client/README.md)

## Navigation

[← Previous Question](./03-decomposition-strategies.md) | [Next Question →](./05-data-consistency.md)
