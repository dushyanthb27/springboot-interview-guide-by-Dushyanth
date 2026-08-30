# What are the anti-patterns in microservices architecture? 🟠

## Question

Identify common anti-patterns in microservices architecture and explain how to avoid them.

## Answer

Common microservices anti-patterns include: Distributed Monolith (services that are so tightly coupled they behave like a monolith), Shared Database (multiple services sharing the same database, defeating the purpose of microservices), Nanoservices (services that are too small and granular, adding overhead without benefits), Synchronous Communication Chains (long chains of synchronous calls causing performance issues), and Vendor Lock-in (heavy dependence on specific cloud providers or technologies).

Other anti-patterns include: Service-oriented Monolith (calling services directly without proper boundaries), Lack of Observability (insufficient monitoring and tracing making debugging difficult), Ignoring Cultural Aspects (implementing microservices without organizational changes), and Premature Optimization (decomposing into microservices before the monolith becomes problematic).

## Code Example

```java
// Anti-Pattern: Distributed Monolith
// Bad: Services tightly coupled through synchronous calls
@Service
public class OrderService {
    @Autowired
    private UserServiceClient userServiceClient;
    @Autowired
    private InventoryServiceClient inventoryClient;
    @Autowired
    private PaymentServiceClient paymentClient;

    @Transactional
    public Order createOrder(Order order) {
        // Tight coupling - all services must be available
        User user = userServiceClient.getUser(order.getUserId());
        inventoryClient.reserveItems(order.getItems());
        paymentClient.processPayment(order.getPayment());
        return orderRepository.save(order);
    }
}

// Good: Event-driven loose coupling
@Service
public class OrderService {
    @Autowired
    private OrderRepository orderRepository;
    @Autowired
    private ApplicationEventPublisher eventPublisher;

    @Transactional
    public Order createOrder(Order order) {
        Order savedOrder = orderRepository.save(order);
        eventPublisher.publishEvent(new OrderCreatedEvent(savedOrder));
        return savedOrder;
    }
}

// Anti-Pattern: Shared Database
// Bad: Multiple services accessing the same database tables
@Service
public class OrderService {
    @Autowired
    private JdbcTemplate jdbcTemplate;

    public Order getOrder(Long id) {
        return jdbcTemplate.queryForObject(
                "SELECT * FROM orders WHERE id = ?", Order.class, id);
    }
}

@Service
public class ReportingService {
    @Autowired
    private JdbcTemplate jdbcTemplate;

    public List<Order> getAllOrders() {
        return jdbcTemplate.query("SELECT * FROM orders", new OrderRowMapper());
    }
}

// Good: Database per service with API communication
@Service
public class ReportingService {
    @Autowired
    private OrderServiceClient orderServiceClient;

    public List<Order> getAllOrders() {
        return orderServiceClient.getAllOrders();
    }
}

// Anti-Pattern: Nanoservices
// Bad: Service too small to provide value
@RestController
public class EmailValidationService {
    @GetMapping("/validate")
    public boolean validateEmail(@RequestParam String email) {
        return email.contains("@");
    }
}

// Good: Combined with other related functionality
@RestController
public class UserService {
    @PostMapping("/users")
    public User createUser(@RequestBody User user) {
        if (!isValidEmail(user.getEmail())) {
            throw new InvalidEmailException();
        }
        return userRepository.save(user);
    }

    private boolean isValidEmail(String email) {
        return email.contains("@");
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your ability to identify architectural problems and your understanding of what not to do when designing microservices. They're looking for awareness of common pitfalls and how to avoid them.

## ⚠️ Common Mistake
Focusing only on patterns without understanding anti-patterns that can undermine microservices benefits. Another mistake is not recognizing when a "microservices" implementation is actually a distributed monolith.

## ⚡ Follow-up Questions

1. How do you identify if your microservices have become a distributed monolith?
2. What are the signs that services are too granular (nanoservices)?
3. How do you avoid shared database anti-pattern?

## 📚 Related Topics

- [Decomposition Strategies](./03-decomposition-strategies.md)
- [Service Boundaries](./07-service-boundaries.md)
- [Communication Patterns](./04-communication-patterns.md)

## Navigation

[← Previous Question](./11-service-authentication.md) | [Next Question →](./13-monitoring-observability.md)
