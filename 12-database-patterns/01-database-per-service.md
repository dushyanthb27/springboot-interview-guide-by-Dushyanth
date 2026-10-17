# What is the database per service pattern? 🟢

## Question

What is the database per service pattern and why is it important in microservices? What are the challenges of this pattern, and how do you handle queries that need data from multiple services?

## Answer

The database per service pattern ensures each microservice has its own database, which only that service can access. This provides loose coupling between services, as changes to one service's database schema don't affect other services. Each service uses the database technology best suited for its needs (SQL, NoSQL, etc.).

**Why database per service is important:**

1. **Loose coupling**: Services can evolve their database schema independently without coordinating with other teams. This enables independent deployment and faster iteration.

2. **Technology diversity**: Each service can choose the database technology that best fits its use case (e.g., PostgreSQL for relational data, MongoDB for document storage, Redis for caching, Elasticsearch for search).

3. **Clear data ownership**: Each team owns their data and is responsible for its integrity. This prevents accidental coupling through shared database access.

4. **Independent scaling**: Each service's database can scale independently based on its specific needs (read-heavy vs write-heavy, data volume, query patterns).

5. **Fault isolation**: Database failures are contained to individual services rather than affecting the entire system.

**Challenges of database per service:**

1. **Cross-service queries**: Cannot join data across services using SQL. Must use API calls or implement alternative patterns.

2. **Distributed transactions**: Cannot use ACID transactions across services. Must implement eventual consistency patterns like Saga.

3. **Data duplication**: Some data may need to be duplicated across services for performance, requiring synchronization logic.

4. **Complex queries**: Complex queries that span multiple business domains become harder to implement.

**Handling cross-service queries:**

1. **API composition**: Service makes API calls to other services to gather data, then composes the response. Simple but may have performance issues for deep aggregations.

2. **Data duplication (CQRS)**: Duplicate data in each service, keeping it in sync through events. Read performance is excellent, but eventual consistency must be accepted.

3. **Event-driven architecture**: When data changes in one service, it publishes an event that other services consume to update their local data.

4. **API Gateway composition**: API Gateway makes parallel calls to multiple services and composes the response, reducing latency compared to sequential calls.

5. **GraphQL federation**: Use GraphQL to combine data from multiple services through a unified schema.

## Code Example

```java
// User Service - PostgreSQL (relational data)
@Service
public class UserService {

    private final UserRepository userRepository;
    private final ApplicationEventPublisher eventPublisher;

    public User createUser(User user) {
        // Save to User Service's own database
        User saved = userRepository.save(user);

        // Publish event for other services to update their data
        eventPublisher.publishEvent(new UserCreatedEvent(saved.getId(), saved.getName()));

        return saved;
    }
}

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    User findByEmail(String email);
}

// Order Service - MongoDB (document data)
@Service
public class OrderService {

    private final OrderRepository orderRepository;

    public Order createOrder(Order order) {
        // Save to Order Service's own database
        return orderRepository.save(order);
    }
}

@Repository
public interface OrderRepository extends MongoRepository<Order, String> {
    List<Order> findByUserId(String userId);
}

// API Composition - fetching data from multiple services
@Service
public class OrderViewService {

    private final OrderServiceClient orderServiceClient;
    private final UserServiceClient userServiceClient;
    private final ProductServiceClient productServiceClient;

    public OrderView getOrderView(String orderId) {
        // Fetch order from Order Service
        Order order = orderServiceClient.getOrder(orderId);

        // Fetch related data from other services
        User user = userServiceClient.getUser(order.getUserId());
        Product product = productServiceClient.getProduct(order.getProductId());

        // Compose the view
        return new OrderView(order, user, product);
    }
}

// Event-driven data synchronization
@Service
public class SearchService {

    @EventListener
    public void handleUserCreated(UserCreatedEvent event) {
        // Update Elasticsearch index for search
        UserDocument doc = new UserDocument(event.getUserId(), event.getUserName());
        elasticsearchRepository.save(doc);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of the database per service pattern and its benefits and challenges. They're looking for awareness that this pattern introduces complexity (no cross-service queries, distributed transactions) but provides significant benefits (loose coupling, independent deployment). A strong answer demonstrates understanding of practical solutions for cross-service queries like API composition and event-driven synchronization.

## ⚠️ Common Mistake

Assuming a shared database is easier and choosing it for microservices. Shared databases tightly couple services through the database schema, defeating the purpose of microservices. Another mistake is not understanding that cross-service queries require different approaches than SQL joins—this is a fundamental shift in thinking for developers transitioning from monolithic to microservices architecture.

## ⚡ Follow-up Questions

1. How do you handle distributed transactions when using database per service?
2. What is the Saga pattern, and how does it handle distributed transactions?
3. When would you use API composition vs event-driven data synchronization?
4. What is the difference between strong consistency and eventual consistency?
5. When would a shared database be acceptable in a microservices architecture?

## 📚 Related Topics

- [Implementing Database per Service](./02-implementing-database-per-service.md)
- [Distributed Data Challenges](./03-distributed-data-challenges.md)
- [02. Microservices Architecture](../02-microservices-architecture/README.md)

## Navigation

[← Main Section](./README.md) | [Next Question →](./02-implementing-database-per-service.md)
