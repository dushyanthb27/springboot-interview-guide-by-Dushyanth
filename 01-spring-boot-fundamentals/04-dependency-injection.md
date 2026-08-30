# Explain dependency injection in Spring Boot 🟡

## Question

What is dependency injection in Spring Boot and what are the different types of dependency injection available?

## Answer

Dependency injection (DI) is a design pattern where dependencies are provided to a class rather than the class creating them, promoting loose coupling and easier testing. Spring Boot implements DI through its IoC (Inversion of Control) container, which manages bean creation and dependency resolution.

The main types of dependency injection in Spring are constructor injection (recommended for mandatory dependencies), setter injection (for optional dependencies), field injection (discouraged due to testing difficulties), and method injection (for complex scenarios). Constructor injection is preferred as it makes dependencies explicit, enables immutability, and facilitates unit testing.

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
