# How do you implement method-level security? 🟡

## Question

How do you implement method-level security in Spring Security?

## Answer

Method-level security in Spring Security is implemented using annotations like @PreAuthorize, @PostAuthorize, @Secured, and @RolesAllowed. These annotations provide fine-grained access control at the method level. Spring Security AOP proxies intercept method calls and enforce security rules based on the annotations.

Implementation includes enabling method security with @EnableMethodSecurity, using annotations on service methods, and configuring custom security expressions for complex authorization logic.

## Code Example

```java
@Configuration
@EnableWebSecurity
@EnableMethodSecurity
public class SecurityConfig {
    // Security filter chain configuration
}

@Service
public class UserService {
    @PreAuthorize("hasRole('ADMIN')")
    public void deleteUser(Long userId) {
        // Delete user logic
    }

    @PreAuthorize("#userId == authentication.principal.id or hasRole('ADMIN')")
    public User getUser(Long userId) {
        // Get user logic
    }

    @PostAuthorize("returnObject.owner == authentication.principal.username")
    public Order getOrder(Long orderId) {
        // Get order logic
    }

    @PreAuthorize("hasAuthority('USER_WRITE')")
    public void updateUser(User user) {
        // Update user logic
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of method-level security and your ability to implement fine-grained authorization.

## ⚠️ Common Mistake
Not enabling @EnableMethodSecurity, which prevents method-level security annotations from working.

## ⚡ Follow-up Questions

1. What is the difference between @PreAuthorize and @PostAuthorize?
2. How do you implement custom security expressions?
3. What is the difference between @Secured and @PreAuthorize?

## 📚 Related Topics

- [RBAC](./05-rbac.md)
- [Securing REST Endpoints](./11-securing-rest-endpoints.md)
- [Custom Security Filters](./12-custom-security-filters.md)

## Navigation

[← Previous Question](./09-cors-handling.md) | [Next Question →](./11-securing-rest-endpoints.md)
