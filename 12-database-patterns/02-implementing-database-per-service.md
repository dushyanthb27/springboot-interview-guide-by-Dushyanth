# How do you implement database per service? 🟡

## Question

How do you implement the database per service pattern in Spring Boot applications?

## Answer

Database per service is implemented by configuring separate datasources for each service, using Spring Data JPA repositories specific to each service's domain, and ensuring services don't directly access each other's databases. Each service has its own database configuration in application.yml or application.properties.

Implementation involves configuring datasource properties, creating entity classes and repositories for each service's domain, and using API calls or messaging to access data from other services instead of direct database access.

## Code Example

```java
// Service A: User Service
@Configuration
public class UserDataSourceConfig {
    @Bean
    @ConfigurationProperties(prefix = "spring.datasource.user")
    public DataSource userDataSource() {
        return DataSourceBuilder.create().build();
    }
}

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
}

// Service B: Order Service
@Configuration
public class OrderDataSourceConfig {
    @Bean
    @ConfigurationProperties(prefix = "spring.datasource.order")
    public DataSource orderDataSource() {
        return DataSourceBuilder.create().build();
    }
}

@Repository
public interface OrderRepository extends JpaRepository<Order, Long> {
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of implementing database per service and your ability to configure multiple datasources.

## ⚠️ Common Mistake
Allowing services to directly access each other's databases, breaking the database per service principle.

## ⚡ Follow-up Questions

1. How do you handle cross-service queries?
2. What are the challenges of multiple datasources?
3. How do you manage database migrations?

## 📚 Related Topics

- [Database per Service](./01-database-per-service.md)
- [Distributed Data Challenges](./03-distributed-data-challenges.md)
- [Database Migrations](./08-database-migrations.md)

## Navigation

[← Previous Question](./01-database-per-service.md) | [Next Question →](./03-distributed-data-challenges.md)
