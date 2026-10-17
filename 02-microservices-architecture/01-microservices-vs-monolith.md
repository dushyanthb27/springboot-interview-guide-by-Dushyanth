# What are microservices and how do they differ from monolithic architecture? 🟡

## Question

You are leading a new platform initiative and have to decide whether to build a high-growth order system as a monolith or split it into microservices. How would you evaluate the decision, describe the trade-offs, and explain when each architecture would be appropriate?

## Answer

Microservices architecture is an approach where applications are structured as a collection of small, autonomous services modeled around business domains. Each service runs in its own process, communicates through well-defined APIs (typically HTTP/REST or messaging), and is independently deployable and scalable. In contrast, monolithic architecture builds the application as a single, unified unit where all components share the same codebase, database, and deployment pipeline.

**Key differences:**

1. **Deployment independence**: Microservices can be deployed individually without affecting other services. Monoliths must be deployed as a single unit, requiring coordination and downtime for updates.

2. **Technology diversity**: Each microservice can use the technology stack best suited for its needs (e.g., Java for one service, Node.js for another). Monoliths typically use a single technology stack across the entire application.

3. **Data management**: Microservices often use database per service pattern, where each service owns its data. Monoliths share a single database, which can lead to tight coupling between modules.

4. **Team organization**: Microservices align with small, autonomous teams (typically 2-10 developers) who own specific business domains. Monoliths often require large teams coordinating changes across a shared codebase.

5. **Failure isolation**: Failures in one microservice are contained and don't bring down the entire system. Monolith failures can affect the entire application.

6. **Scalability**: Microservices can scale individual components based on demand. Monoliths must scale the entire application, even if only one component needs more resources.

**Trade-offs:**

Microservices introduce complexity in distributed systems: service discovery, inter-service communication, distributed transactions, eventual consistency, monitoring across services, and deployment automation. Monoliths are simpler to develop initially but become harder to maintain as the application grows.

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

The interviewer wants to test your understanding of fundamental architectural patterns and your ability to articulate the trade-offs between different approaches. They're looking for awareness that microservices aren't always the right answer—you should demonstrate critical thinking about when to use each approach. A strong answer acknowledges that microservices introduce complexity and should be chosen based on specific organizational and technical needs, not as a default.

## ⚠️ Common Mistake

Assuming microservices are always better than monoliths. This is a common misconception—microservices introduce significant complexity (distributed transactions, service discovery, monitoring, deployment automation) that may not be justified for small teams or simple applications. Another mistake is not acknowledging that many successful companies run large monoliths and that migration to microservices should be driven by specific pain points, not trends.

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
