# How do you customize Spring Data REST endpoints? 🟡

## Question

What are the different ways to customize Spring Data REST endpoints and responses?

## Answer

Spring Data REST endpoints can be customized through several approaches: using @RepositoryRestResource and @RestResource annotations to control exposure, implementing RepositoryRestConfigurer to configure global settings, creating custom controllers to supplement repository endpoints, using event handlers to modify responses, and implementing custom validators.

Customization options include changing base paths, hiding specific methods or repositories, adding custom query methods, implementing CORS configuration, customizing serialization/deserialization, and adding business logic through event handlers. For complex customizations, you can mix Spring Data REST with traditional Spring MVC controllers.

## Code Example

```java
package com.example.demo.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.data.rest.core.config.RepositoryRestConfiguration;
import org.springframework.data.rest.webmvc.config.RepositoryRestConfigurer;
import org.springframework.http.HttpMethod;
import org.springframework.web.servlet.config.annotation.CorsRegistry;

@Configuration
public class RestConfig implements RepositoryRestConfigurer {

    @Override
    public void configureRepositoryRestConfiguration(RepositoryRestConfiguration config) {
        // Expose IDs for all entities
        config.exposeIdsFor(User.class, Order.class);

        // Set base URI
        config.setBasePath("/api");

        // Configure CORS
        config.getCorsRegistry()
                .addMapping("/**")
                .allowedOrigins("http://localhost:3000")
                .allowedMethods("GET", "POST", "PUT", "DELETE", "PATCH");
    }

    @Override
    public void configureRepositoryRestConfiguration(RepositoryRestConfiguration config, CorsRegistry cors) {
        // Disable specific HTTP methods
        config.getCorsRegistry().addMapping("/api/users/**")
                .allowedMethods(HttpMethod.GET, HttpMethod.POST);
    }
}

// Custom controller to supplement repository endpoints
@RestController
@RequestMapping("/api/users")
public class UserController {

    @Autowired
    private UserRepository userRepository;

    @PostMapping("/{id}/activate")
    public ResponseEntity<User> activateUser(@PathVariable Long id) {
        User user = userRepository.findById(id)
                .orElseThrow(() -> new UserNotFoundException(id));
        user.setActive(true);
        return ResponseEntity.ok(userRepository.save(user));
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Spring Data REST customization capabilities and your ability to go beyond the default behavior. They're looking for knowledge of different customization approaches.

## ⚠️ Common Mistake
Over-customizing Spring Data REST endpoints to the point where you lose the benefits of automatic endpoint generation. Another mistake is not understanding when to use custom controllers instead of trying to force everything into Spring Data REST.

## ⚡ Follow-up Questions

1. How do you add custom business logic to Spring Data REST operations?
2. What is the difference between RepositoryRestConfigurer and custom controllers?
3. How do you implement custom validation in Spring Data REST?

## 📚 Related Topics

- [Creating Repositories](./02-creating-repositories.md)
- [Event Handling](./07-event-handling.md)
- [Custom Controllers](./12-custom-controllers.md)

## Navigation

[← Previous Question](./03-repository-projections.md) | [Next Question →](./05-hateoas.md)
