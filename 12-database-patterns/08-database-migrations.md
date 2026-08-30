# How do you handle database migrations in microservices? 🟡

## Question

How do you manage database schema migrations in a microservices architecture?

## Answer

Database migrations in microservices are managed per service using tools like Flyway or Liquibase. Each service manages its own database schema independently, with migration scripts versioned and applied automatically on startup. This ensures each service's database schema is in sync with its code.

Implementation involves adding Flyway or Liquibase dependency, creating migration scripts in the db/migration directory, and configuring the migration tool to run on application startup.

## Code Example

```java
// Dependency
<dependency>
    <groupId>org.flywaydb</groupId>
    <artifactId>flyway-core</artifactId>
</dependency>

// Migration script: V1__Create_users_table.sql
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    username VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

// Migration script: V2__Add_status_column.sql
ALTER TABLE users ADD COLUMN status VARCHAR(50) DEFAULT 'ACTIVE';

// application.yml
spring:
  flyway:
    enabled: true
    locations: classpath:db/migration
    baseline-on-migrate: true
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of database migration management in microservices and your ability to use migration tools.

## ⚠️ Common Mistake
Not versioning migration scripts properly, leading to inconsistent database states across environments.

## ⚡ Follow-up Questions

1. How do you handle rollback of migrations?
2. What is the difference between Flyway and Liquibase?
3. How do you manage migrations in production?

## 📚 Related Topics

- [Database per Service](./01-database-per-service.md)
- [Database Best Practices](./13-database-best-practices.md)
- [14. Deployment with Docker and Kubernetes](../14-deployment-docker-kubernetes/README.md)

## Navigation

[← Previous Question](./07-read-replicas.md) | [Next Question →](./09-database-sharding.md)
