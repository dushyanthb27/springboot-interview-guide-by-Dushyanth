# How do you implement database security? 🟡

## Question

How do you implement security for database access in Spring Boot applications?

## Answer

Database security involves using least privilege access, encrypting sensitive data, implementing connection security (SSL/TLS), using parameterized queries to prevent SQL injection, and properly managing database credentials. Spring Boot provides configuration for datasource security and connection pooling.

Implementation includes configuring datasource credentials securely (using environment variables or secrets management), enabling SSL for database connections, using Spring Data JPA's parameterized queries, and implementing proper access controls.

## Code Example

```java
// Secure datasource configuration
@Configuration
public class DataSourceConfig {
    @Bean
    public DataSource dataSource() {
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl("jdbc:postgresql://localhost:5432/userdb");
        config.setUsername("${DB_USERNAME}");
        config.setPassword("${DB_PASSWORD}");
        config.setConnectionTestQuery("SELECT 1");
        return new HikariDataSource(config);
    }
}

// Use parameterized queries (automatic with Spring Data JPA)
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    @Query("SELECT u FROM User u WHERE u.email = :email")
    User findByEmail(@Param("email") String email);
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of database security and your ability to implement secure database access.

## ⚠️ Common Mistake
Hardcoding database credentials in configuration files, creating security vulnerabilities.

## ⚡ Follow-up Questions

1. How do you manage database credentials securely?
2. What is SQL injection and how do you prevent it?
3. How do you enable SSL for database connections?

## 📚 Related Topics

- [09. Security with OAuth2](../09-security-oauth2/README.md)
- [Multi-Tenancy](./11-multi-tenancy.md)
- [Database Best Practices](./13-database-best-practices.md)

## Navigation

[← Previous Question](./13-database-best-practices.md) | [Back to Section](./README.md)
