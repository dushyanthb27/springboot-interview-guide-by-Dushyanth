# How do you handle events in Spring Data REST? 🟡

## Question

What are the different types of events in Spring Data REST and how do you handle them?

## Answer

Spring Data REST provides event handling through Spring's ApplicationEventPublisher mechanism. Events are fired during repository operations like before/after create, save, delete, and other lifecycle events. You can handle these events by creating event listener methods annotated with @EventListener or implementing ApplicationListener interfaces.

Common events include BeforeCreateEvent, AfterCreateEvent, BeforeSaveEvent, AfterSaveEvent, BeforeDeleteEvent, AfterDeleteEvent, and BeforeLinkSaveEvent. Event handlers are useful for validation, auditing, sending notifications, or triggering business logic during repository operations.

## Code Example

```java
@Component
public class UserEventHandler {

    @HandleBeforeCreate
    public void handleBeforeCreate(User user) {
        // Validation before creation
        if (user.getEmail() == null || !user.getEmail().contains("@")) {
            throw new InvalidEmailException("Invalid email format");
        }
        user.setCreatedAt(LocalDateTime.now());
    }

    @HandleAfterCreate
    public void handleAfterCreate(User user) {
        // Send welcome email after user creation
        emailService.sendWelcomeEmail(user);
    }

    @HandleBeforeSave
    public void handleBeforeSave(User user) {
        // Update timestamp before save
        user.setUpdatedAt(LocalDateTime.now());
    }

    @HandleAfterSave
    public void handleAfterSave(User user) {
        // Update search index
        searchService.indexUser(user);
    }

    @HandleBeforeDelete
    public void handleBeforeDelete(User user) {
        // Check if user can be deleted
        if (user.hasActiveOrders()) {
            throw new UserDeletionException("Cannot delete user with active orders");
        }
    }

    @HandleAfterDelete
    public void handleAfterDelete(User user) {
        // Clean up related data
        cleanupService.cleanupUserData(user);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Spring Data REST's event mechanism and your ability to implement cross-cutting concerns during repository operations.

## ⚠️ Common Mistake
Performing long-running operations in event handlers which can slow down repository operations. Another mistake is not handling exceptions properly in event handlers.

## ⚡ Follow-up Questions

1. What is the difference between @HandleBeforeCreate and @HandleAfterCreate?
2. How do you handle events asynchronously in Spring Data REST?
3. Can you modify the entity in event handlers?

## 📚 Related Topics

- [Customizing Endpoints](./04-customizing-endpoints.md)
- [Validation](./09-validation.md)
- [Custom Controllers](./12-custom-controllers.md)

## Navigation

[← Previous Question](./06-paging-sorting.md) | [Next Question →](./08-security.md)
