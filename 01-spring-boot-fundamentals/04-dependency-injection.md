# Explain dependency injection in Spring Boot 🟡

## Question

What is dependency injection in Spring Boot and what are the different types of dependency injection available? Which type is recommended and why?

## Answer

Dependency injection (DI) is a design pattern where dependencies are provided to a class rather than the class creating them, promoting loose coupling and easier testing. Spring Boot implements DI through its IoC (Inversion of Control) container, which manages bean creation and dependency resolution. The container is responsible for instantiating, configuring, and assembling beans.

**Types of dependency injection in Spring:**

1. **Constructor Injection (Recommended)**: Dependencies are provided through the class constructor. This is the preferred approach for mandatory dependencies because:
   - Makes dependencies explicit and required
   - Enables immutability (dependencies can be final)
   - Facilitates unit testing (can easily pass mocks without Spring context)
   - Ensures the object is always in a valid state after construction
   - Works well with Java's final fields

2. **Setter Injection**: Dependencies are provided through setter methods. Used for optional dependencies that may not be required for the object to function. Less preferred because:
   - Dependencies can be null, requiring null checks
   - Doesn't guarantee immutability
   - Object may be in invalid state between construction and dependency injection

3. **Field Injection (Discouraged)**: Dependencies are injected directly into fields using @Autowired. This approach is discouraged because:
   - Makes dependencies implicit (not visible in constructor)
   - Harder to test (requires reflection or Spring TestContext)
   - Prevents fields from being final
   - Hides dependencies from the class API
   - Can lead to tight coupling to Spring

4. **Method Injection**: Used for complex scenarios where dependencies need to be injected into non-setter methods. Rarely needed in typical applications.

**Why constructor injection is preferred:**
- Clear intent: Required dependencies are visible in the constructor
- Immutability: Dependencies can be declared final
- Testability: Easy to instantiate with mock dependencies
- Validation: Constructor validates dependencies at object creation time
- IDE support: IDEs can help generate constructors automatically

## Code Example

```java
package com.example.demo.service;

import org.springframework.stereotype.Service;

@Service
public class OrderService {
    private final UserService userService;
    private final PaymentService paymentService;
    private NotificationService notificationService; // Optional dependency

    // Constructor injection - recommended for mandatory dependencies
    public OrderService(UserService userService, PaymentService paymentService) {
        this.userService = userService;
        this.paymentService = paymentService;
    }

    // Setter injection - for optional dependencies
    public void setNotificationService(NotificationService notificationService) {
        this.notificationService = notificationService;
    }

    public Order createOrder(Order order) {
        User user = userService.getUser(order.getUserId());
        paymentService.processPayment(order.getPayment());
        if (notificationService != null) {
            notificationService.sendOrderConfirmation(order);
        }
        return order;
    }
}

// Field injection - discouraged
@Service
public class BadExampleService {
    @Autowired
    private UserRepository userRepository; // Hard to test, not explicit
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of the core Spring concept and your ability to choose the appropriate injection method. They're looking for awareness of best practices and the trade-offs between different injection types.

## ⚠️ Common Mistake

Using field injection (@Autowired on fields) instead of constructor injection, which makes dependencies implicit, harder to test, and prevents making fields final. Another mistake is not understanding when to use setter injection for optional dependencies.

## ⚡ Follow-up Questions

1. Why is constructor injection preferred over field injection?
2. How does Spring resolve circular dependencies?
3. What is the difference between @Autowired and @Inject?

## 📚 Related Topics

- [Dependency Injection Methods](./07-dependency-injection-methods.md)
- [Bean Configuration](./13-bean-configuration.md)
- [Bean Scopes](./14-bean-scopes.md)

## Navigation

[← Previous Question](./03-stereotype-annotations.md) | [Next Question →](./05-spring-boot-starters.md)
