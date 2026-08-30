# How do you integrate Spring Data REST with custom controllers? 🟠

## Question

How can you combine Spring Data REST repositories with custom REST controllers in the same application?

## Answer

Spring Data REST can be integrated with custom Spring MVC controllers by placing them in the same application, using different base paths to avoid conflicts, and leveraging Spring Data REST's RepositoryRestResource alongside @RestController controllers. Custom controllers can supplement Spring Data REST endpoints or provide functionality that doesn't fit the repository pattern.

Integration approaches include using different base paths (e.g., /api for Spring Data REST, /custom for custom controllers), implementing custom controllers that work with the same repositories, using @RepositoryRestController for controllers that integrate with Spring Data REST's configuration, and sharing common service logic between both approaches.

## Code Example

```java
// Spring Data REST repository
@RepositoryRestResource(path = "users", collectionResourceRel = "users")
public interface UserRepository extends JpaRepository<User, Long> {
    // Automatic CRUD endpoints at /api/users
}

// Custom controller with different base path
@RestController
@RequestMapping("/custom/users")
public class CustomUserController {

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

// RepositoryRestController for tighter integration
@RepositoryRestController
@RequestMapping("/users")
public class UserExtensionController {

    @Autowired
    private UserRepository userRepository;

    @PostMapping("/{id}/deactivate")
    public ResponseEntity<User> deactivateUser(@PathVariable Long id) {
        User user = userRepository.findById(id)
                .orElseThrow(() -> new UserNotFoundException(id));
        user.setActive(false);
        return ResponseEntity.ok(userRepository.save(user));
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of mixing Spring Data REST with custom controllers and your ability to design hybrid API architectures.

## ⚠️ Common Mistake
Creating path conflicts between Spring Data REST and custom controllers. Another mistake is not understanding the different integration approaches and their trade-offs.

## ⚡ Follow-up Questions

1. What is the difference between @RestController and @RepositoryRestController?
2. How do you avoid path conflicts between Spring Data REST and custom controllers?
3. Can you share the same entity between Spring Data REST and custom controllers?

## 📚 Related Topics

- [Customizing Endpoints](./04-customizing-endpoints.md)
- [Limitations](./11-limitations.md)
- [08. Spring Rest Client](../08-spring-rest-client/README.md)

## Navigation

[← Previous Question](./11-limitations.md) | [Back to Section](./README.md)
