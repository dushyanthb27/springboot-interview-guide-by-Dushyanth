# What is the database per service pattern? 🟢

## Question

What is the database per service pattern and why is it important in microservices?

## Answer

The database per service pattern ensures each microservice has its own database, which only that service can access. This provides loose coupling between services, as changes to one service's database schema don't affect other services. Each service uses the database technology best suited for its needs (SQL, NoSQL, etc.).

Importance includes: independent service deployment, technology diversity, clear data ownership, and reduced coordination between teams. However, it introduces challenges for queries that need data from multiple services.

## Code Example

```java
// Service A: User Service with PostgreSQL
@Service
public class UserService {
    private final UserRepository userRepository;
    // Only User Service accesses this database
}

// Service B: Order Service with MongoDB
@Service
public class OrderService {
    private final OrderRepository orderRepository;
    // Only Order Service accesses this database
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of the database per service pattern and its benefits and challenges.

## ⚡ Follow-up Questions

1. What are the challenges of database per service?
2. How do you handle queries across multiple services?
3. When would you use a shared database instead?

## 📚 Related Topics

- [Implementing Database per Service](./02-implementing-database-per-service.md)
- [Distributed Data Challenges](./03-distributed-data-challenges.md)
- [02. Microservices Architecture](../02-microservices-architecture/README.md)

## Navigation

[← Main Section](./README.md) | [Next Question →](./02-implementing-database-per-service.md)
