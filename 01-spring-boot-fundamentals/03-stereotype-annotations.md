# What is the difference between @Component, @Service, @Repository, and @Controller? 🟢

## Question

Explain the differences between Spring's stereotype annotations (@Component, @Service, @Repository, @Controller) and when to use each one.

## Answer

These are all meta-annotations that mark classes as Spring-managed components (beans), but they serve different semantic purposes. @Component is the generic stereotype for any Spring-managed component. @Service indicates a service layer component containing business logic. @Repository indicates a data access layer component and enables exception translation for database operations. @Controller indicates a presentation layer component for Spring MVC web applications.

While functionally similar (all make classes eligible for component scanning), using the appropriate stereotype provides semantic meaning to your application architecture and enables framework-specific behavior like exception translation for repositories or request mapping for controllers.

## Code Example

```java
package com.example.demo;

import org.springframework.stereotype.Component;
import org.springframework.stereotype.Service;
import org.springframework.stereotype.Repository;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

// Generic component - use when other stereotypes don't fit
@Component
public class EmailValidator {
    public boolean validate(String email) {
        return email.contains("@");
    }
}

// Service layer - business logic
@Service
public class UserService {
    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User createUser(User user) {
        return userRepository.save(user);
    }
}

// Repository layer - data access with exception translation
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    User findByEmail(String email);
}

// Controller layer - web endpoints
@Controller
public class UserController {
    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/users")
    public String listUsers(Model model) {
        model.addAttribute("users", userService.getAllUsers());
        return "users";
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Spring's layered architecture and semantic annotations. They're looking for awareness that while these annotations are functionally similar, they provide architectural structure and enable framework-specific behaviors.

## ⚡ Follow-up Questions

1. What is the purpose of @RestController compared to @Controller?
2. Can you use @Component instead of the specific stereotypes?
3. How does exception translation work with @Repository?

## 📚 Related Topics

- [Dependency Injection](./04-dependency-injection.md)
- [Bean Configuration](./13-bean-configuration.md)
- [03. Spring Data REST](../03-spring-data-rest/README.md)

## Navigation

[← Previous Question](./02-auto-configuration.md) | [Next Question →](./04-dependency-injection.md)
