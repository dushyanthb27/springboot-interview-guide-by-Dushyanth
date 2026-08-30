# How do you design service boundaries in microservices? 🟠

## Question

What are the principles and approaches for defining service boundaries in microservices architecture?

## Answer

Service boundaries should be designed around business capabilities using Domain-Driven Design (DDD) principles, specifically bounded contexts. Key principles include aligning services with business domains (not technical layers), ensuring high cohesion within services (related functionality together), minimizing coupling between services (well-defined interfaces), and matching team structure to service boundaries (Conway's Law).

Approaches include business capability decomposition (identify distinct business capabilities), domain-driven design bounded contexts (identify domains and their boundaries), data ownership (services own their data), and volatility-based decomposition (separate frequently changing parts). Service boundaries should evolve as understanding of the domain grows and should be based on organizational structure and communication patterns.

## Code Example

```java
// Business Capability-Based Service Boundaries
// User Management Service
@DomainContext("user-management")
@SpringBootApplication
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}

@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String email;
    private String password;
    // User-related data only
}

// Order Management Service
@DomainContext("order-management")
@SpringBootApplication
public class OrderServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(OrderServiceApplication.class, args);
    }
}

@Entity
@Table(name = "orders")
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private Long userId; // Reference to user, not full user object
    private BigDecimal amount;
    private OrderStatus status;
    // Order-related data only
}

// Bounded Context with Anti-Corruption Layer
@Service
public class OrderService {
    @Autowired
    private UserRepository userRepository;

    public OrderDTO createOrder(OrderRequest request) {
        // Anti-corruption layer - convert external request to internal model
        User user = userRepository.findById(request.getUserId())
                .orElseThrow(() -> new UserNotFoundException(request.getUserId()));

        Order order = new Order();
        order.setUserId(user.getId());
        order.setAmount(request.getAmount());
        order.setStatus(OrderStatus.PENDING);

        Order savedOrder = orderRepository.save(order);

        // Convert internal model to external DTO
        return convertToDTO(savedOrder);
    }

    private OrderDTO convertToDTO(Order order) {
        OrderDTO dto = new OrderDTO();
        dto.setId(order.getId());
        dto.setAmount(order.getAmount());
        dto.setStatus(order.getStatus());
        return dto;
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of service design principles and your ability to apply Domain-Driven Design concepts. They're looking for awareness that service boundaries should be based on business domains rather than technical considerations.

## ⚠️ Common Mistake

Defining service boundaries based on technical layers (user controller, user service, user repository) rather than business capabilities. Another mistake is making services too granular (nanoservices) which increases overhead without benefits.

## ⚡ Follow-up Questions

1. What is the relationship between bounded contexts and microservices?
2. How do you handle shared data between services with different boundaries?
3. What is an anti-corruption layer and when would you use it?

## 📚 Related Topics

- [Decomposition Strategies](./03-decomposition-strategies.md)
- [Communication Patterns](./04-communication-patterns.md)
- [Data Consistency](./05-data-consistency.md)

## Navigation

[← Previous Question](./06-cap-theorem.md) | [Next Question →](./08-deployment-patterns.md)
