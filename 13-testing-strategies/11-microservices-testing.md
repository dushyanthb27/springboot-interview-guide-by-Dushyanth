# How do you test microservices integration? 🟠

## Question

How do you test integration between multiple microservices?

## Answer

Microservices integration testing involves testing how services work together. Approaches include using Testcontainers to spin up dependent services, using service virtualization tools like WireMock to mock external services, and implementing end-to-end tests that test complete workflows across services.

Implementation involves configuring test environments with required services, using service virtualization for unavailable dependencies, and testing complete user flows across service boundaries.

## Code Example

```java
@SpringBootTest
@Testcontainers
class OrderServiceIntegrationTest {
    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:14");

    @Container
    static RabbitMQContainer rabbitmq = new RabbitMQContainer("rabbitmq:3-management");

    @Autowired
    private OrderService orderService;

    @Test
    void testCreateOrderFlow() {
        // Test complete flow across services
        Order order = orderService.createOrder(new Order(1L, 100));
        assertNotNull(order);
        // Verify downstream services received events
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of microservices integration testing and your ability to test cross-service interactions.

## ⚠️ Common Mistake
Not isolating tests properly, leading to flaky tests due to external dependencies.

## ⚡ Follow-up Questions

1. How do you handle external service dependencies in tests?
2. What is service virtualization?
3. How do you ensure test isolation in microservices testing?

## 📚 Related Topics

- [Testcontainers](./06-testcontainers.md)
- [Contract Testing](./10-contract-testing.md)
- [02. Microservices Architecture](../02-microservices-architecture/README.md)

## Navigation

[← Previous Question](./10-contract-testing.md) | [Next Question →](./12-performance-testing.md)
