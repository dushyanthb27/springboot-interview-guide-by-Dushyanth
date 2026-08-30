# What are the limitations of Spring Data REST? 🟡

## Question

What are the main limitations of Spring Data REST and when should you consider using custom REST controllers instead?

## Answer

Spring Data REST has several limitations including limited control over endpoint structure (convention over configuration), difficulty implementing complex business logic in endpoints, limited support for complex query parameters, challenges with custom validation and error handling, and reduced flexibility for non-CRUD operations. It's best suited for simple CRUD operations and may not be appropriate for complex APIs.

When to use custom controllers instead: complex business logic that doesn't fit repository operations, non-standard API design requirements, complex query parameters and filtering, custom response formats beyond projections, operations that involve multiple entities, and when you need fine-grained control over the API structure.

## Code Example

```java
// Spring Data REST is great for simple CRUD:
@RepositoryRestResource(path = "users")
public interface UserRepository extends JpaRepository<User, Long> {
    // Automatic CRUD endpoints
}

// But use custom controllers for complex operations:
@RestController
@RequestMapping("/api/users")
public class UserController {

    @PostMapping("/{id}/complex-operation")
    public ResponseEntity<User> performComplexOperation(
            @PathVariable Long id,
            @RequestBody ComplexRequest request) {
        // Complex business logic involving multiple services
        User user = userService.getUser(id);
        orderService.processOrders(user, request.getOrders());
        notificationService.sendNotifications(user, request.getNotifications());
        return ResponseEntity.ok(user);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of when to use Spring Data REST versus custom controllers and your ability to make architectural decisions based on requirements.

## ⚠️ Common Mistake
Using Spring Data REST for complex APIs that would be better served by custom controllers. Another mistake is trying to force complex business logic into Spring Data REST's conventions.

## ⚡ Follow-up Questions

1. How do you decide between Spring Data REST and custom controllers?
2. Can you mix Spring Data REST with custom controllers?
3. What are the signs that Spring Data REST is not the right choice?

## 📚 Related Topics

- [Custom Controllers](./12-custom-controllers.md)
- [Customizing Endpoints](./04-customizing-endpoints.md)
- [08. Spring Rest Client](../08-spring-rest-client/README.md)

## Navigation

[← Previous Question](./10-json-customization.md) | [Next Question →](./12-custom-controllers.md)
