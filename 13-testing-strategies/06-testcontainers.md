# How do you test with Testcontainers? 🟠

## Question

How do you use Testcontainers for integration testing with real infrastructure?

## Answer

Testcontainers provides lightweight, throwaway instances of databases, message brokers, and other infrastructure for integration tests. It runs actual Docker containers during tests, providing more realistic testing than embedded databases. Testcontainers supports PostgreSQL, MySQL, RabbitMQ, Kafka, and many other services.

Implementation involves adding Testcontainers dependency, using @Container annotation for container lifecycle management, and configuring the application to use the container's connection details.

## Code Example

```java
@Testcontainers
@SpringBootTest
class OrderServiceIntegrationTest {
    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:14")
            .withDatabaseName("testdb")
            .withUsername("test")
            .withPassword("test");

    @DynamicPropertySource
    static void postgresProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }

    @Test
    void testCreateOrder() {
        // Test with real PostgreSQL
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Testcontainers and your ability to test with real infrastructure.

## ⚠️ Common Mistake
Not cleaning up test data between tests, leading to test pollution.

## ⚡ Follow-up Questions

1. What are the advantages of Testcontainers over embedded databases?
2. How do you configure multiple containers?
3. How do Testcontainers affect test performance?

## 📚 Related Topics

- [Integration Testing](./03-integration-testing.md)
- [Repository Testing](./05-repository-testing.md)
- [Microservices Testing](./11-microservices-testing.md)

## Navigation

[← Previous Question](./05-repository-testing.md) | [Next Question →](./07-messaging-testing.md)
