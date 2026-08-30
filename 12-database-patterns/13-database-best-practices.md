# What are the best practices for database design in microservices? 🟡

## Question

What are the best practices for database design in microservices architecture?

## Answer

Best practices include: using database per service for loose coupling, choosing the right database technology for each service (SQL vs NoSQL), implementing proper indexing for performance, using database migrations for schema management, implementing caching strategies, and monitoring database performance.

Additional practices include designing for eventual consistency, implementing proper backup and recovery strategies, using connection pooling, and following database normalization principles where appropriate.

## Code Example

```java
// Best practice: Use database per service
@Service
public class UserService {
    private final UserRepository userRepository;
    // Only accesses user database
}

// Best practice: Use connection pooling
@Configuration
public class DataSourceConfig {
    @Bean
    public HikariDataSource dataSource() {
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl("jdbc:postgresql://localhost:5432/userdb");
        config.setMaximumPoolSize(20);
        config.setMinimumIdle(5);
        return new HikariDataSource(config);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of database design best practices and your ability to design scalable database architectures.

## ⚠️ Common Mistake
Sharing databases between services, creating tight coupling and deployment dependencies.

## ⚡ Follow-up Questions

1. How do you choose between SQL and NoSQL?
2. What are the database performance optimization techniques?
3. How do you design for scalability?

## 📚 Related Topics

- [Database per Service](./01-database-per-service.md)
- [Caching Strategies](./10-caching-strategies.md)
- [Database Migrations](./08-database-migrations.md)

## Navigation

[← Previous Question](./12-distributed-transactions.md) | [Next Question →](./14-database-security.md)
