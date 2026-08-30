# What are microservices and how do they differ from monolithic architecture? 🟡

## Question

Explain the microservices architecture pattern and compare it with traditional monolithic architecture, highlighting the key differences and trade-offs.

## Answer

Microservices architecture is an approach where applications are structured as a collection of small, autonomous services modeled around business domains. Each service runs in its own process, communicates through well-defined APIs (typically HTTP/REST or messaging), and is independently deployable and scalable. In contrast, monolithic architecture builds the application as a single, unified unit where all components share the same codebase, database, and deployment pipeline.

Key differences include: deployment independence (microservices can be deployed individually vs monolith as one unit), technology diversity (microservices can use different technologies vs monolith typically uses one stack), data management (microservices often use database per service vs monolith shares one database), team organization (microservices align with small autonomous teams vs monolith often has large teams), and failure isolation (microservices contain failures to individual services vs monolith failures can affect the entire application).

## Code Example

```java
// Monolithic Architecture Example
@SpringBootApplication
public class MonolithicApplication {
    public static void main(String[] args) {
        SpringApplication.run(MonolithicApplication.class, args);
    }
}

@RestController
@RequestMapping("/api")
public class MonolithicController {
    @Autowired
    private UserService userService;
    @Autowired
    private OrderService orderService;
    @Autowired
    private PaymentService paymentService;
    @Autowired
    private InventoryService inventoryService;

    // All services in one application
    @PostMapping("/orders")
    public Order createOrder(@RequestBody Order order) {
        // Tight coupling between services
        User user = userService.getUser(order.getUserId());
        paymentService.processPayment(order.getPayment());
        inventoryService.reserveItems(order.getItems());
        return orderService.save(order);
    }
}

// Microservices Architecture Example
// User Service
@SpringBootApplication
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}

@RestController
@RequestMapping("/api/users")
public class UserController {
    @Autowired
    private UserService userService;

    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }
}

// Order Service (separate application)
@SpringBootApplication
public class OrderServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(OrderServiceApplication.class, args);
    }
}

@RestController
@RequestMapping("/api/orders")
public class OrderController {
    @Autowired
    private OrderService orderService;
    @Autowired
    private WebClient webClient; // For service communication

    @PostMapping
    public Order createOrder(@RequestBody Order order) {
        // Communicate with other services via HTTP
        User user = webClient.get()
                .uri("http://user-service/api/users/" + order.getUserId())
                .retrieve()
                .bodyToMono(User.class)
                .block();

        order.setUserName(user.getName());
        return orderService.save(order);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of fundamental architectural patterns and your ability to articulate the trade-offs between different approaches. They're looking for awareness of when to choose microservices over monoliths and the implications of that decision.

## ⚠️ Common Mistake

Thinking microservices are always better than monoliths without considering the complexity overhead. Another mistake is not understanding that microservices introduce distributed systems challenges that don't exist in monoliths.

## ⚡ Follow-up Questions

1. When would you choose a monolithic architecture over microservices?
2. What are the signs that a monolith should be decomposed into microservices?
3. How do you handle shared functionality between microservices?

## 📚 Related Topics

- [Decomposition Strategies](./03-decomposition-strategies.md)
- [Communication Patterns](./04-communication-patterns.md)
- [01. Spring Boot Fundamentals](../01-spring-boot-fundamentals/README.md)

## Navigation

[← Main Section](./README.md) | [Next Question →](./02-benefits-challenges.md)
