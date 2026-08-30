# What is the difference between @Autowired and constructor injection? 🟡

## Question

Compare @Autowired annotation with constructor injection and explain which approach is recommended and why.

## Answer

@Autowired is an annotation that can be used for constructor, field, or setter injection, while constructor injection is a specific DI pattern where dependencies are provided through the constructor. In Spring Boot 4.3+, @Autowired is optional for constructor injection if there's a single constructor. Constructor injection is recommended over @Autowired field injection because it makes dependencies explicit, enables immutability with final fields, facilitates unit testing (no need for reflection), and ensures required dependencies are always provided.

@Autowired field injection should be avoided as it hides dependencies, makes testing difficult, prevents fields from being final, and can lead to NullPointerException if the container fails to inject dependencies.

## Code Example

```java
package com.example.demo.service;

import org.springframework.stereotype.Service;
import org.springframework.beans.factory.annotation.Autowired;

// Recommended: Constructor injection without @Autowired (Spring 4.3+)
@Service
public class OrderService {
    private final UserService userService;
    private final PaymentService paymentService;

    // No @Autowired needed when there's a single constructor
    public OrderService(UserService userService, PaymentService paymentService) {
        this.userService = userService;
        this.paymentService = paymentService;
    }

    public Order createOrder(Order order) {
        User user = userService.getUser(order.getUserId());
        paymentService.processPayment(order.getPayment());
        return order;
    }
}

// Also acceptable: Constructor injection with @Autowired (older Spring versions)
@Service
public class LegacyOrderService {
    private final UserService userService;
    private final PaymentService paymentService;

    @Autowired
    public LegacyOrderService(UserService userService, PaymentService paymentService) {
        this.userService = userService;
        this.paymentService = paymentService;
    }
}

// Discouraged: Field injection with @Autowired
@Service
public class BadOrderService {
    @Autowired
    private UserService userService; // Hidden dependency, hard to test

    @Autowired
    private PaymentService paymentService; // Can't be final

    public Order createOrder(Order order) {
        return order;
    }
}

// Setter injection with @Autowired (for optional dependencies)
@Service
public class NotificationService {
    private EmailSender emailSender;

    @Autowired
    public void setEmailSender(EmailSender emailSender) {
        this.emailSender = emailSender;
    }

    public void sendNotification(String message) {
        if (emailSender != null) {
            emailSender.send(message);
        }
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of dependency injection best practices and your ability to choose the appropriate injection method. They're looking for awareness of why constructor injection is preferred and the problems with field injection.

## ⚠️ Common Mistake

Using @Autowired field injection instead of constructor injection, which makes dependencies implicit and harder to test. Another mistake is not understanding that @Autowired is optional for single-constructor injection in modern Spring versions.

## ⚡ Follow-up Questions

1. How does Spring handle circular dependencies with constructor injection?
2. When would you use setter injection instead of constructor injection?
3. What is the difference between @Autowired and @Inject?

## 📚 Related Topics

- [Dependency Injection](./04-dependency-injection.md)
- [Bean Configuration](./13-bean-configuration.md)
- [Bean Scopes](./14-bean-scopes.md)

## Navigation

[← Previous Question](./06-application-properties.md) | [Next Question →](./08-application-lifecycle.md)
