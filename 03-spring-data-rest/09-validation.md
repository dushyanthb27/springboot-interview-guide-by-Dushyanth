# How do you validate data in Spring Data REST? 🟡

## Question

What are the approaches to implement validation in Spring Data REST endpoints?

## Answer

Validation in Spring Data REST can be implemented using Bean Validation annotations (JSR-380) on entity classes, custom validators using the Validator interface, event handlers for validation logic, and method-level validation. Spring Data REST automatically validates entities on create and update operations if validation is enabled.

Validation approaches include adding @Valid, @NotNull, @Size, @Email, and other Bean Validation annotations to entity fields, implementing custom validators for complex validation logic, using @HandleBeforeCreate and @HandleBeforeSave event handlers for validation, and configuring global validation settings.

## Code Example

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @NotBlank(message = "Name is required")
    @Size(min = 2, max = 50, message = "Name must be between 2 and 50 characters")
    private String name;

    @NotBlank(message = "Email is required")
    @Email(message = "Invalid email format")
    private String email;

    @Min(value = 18, message = "Age must be at least 18")
    private Integer age;
}

// Custom validator for complex validation
@Component
public class UserValidator implements Validator {

    @Override
    public boolean supports(Class<?> clazz) {
        return User.class.isAssignableFrom(clazz);
    }

    @Override
    public void validate(Object target, Errors errors) {
        User user = (User) target;
        if (user.getEmail() != null && userRepository.existsByEmail(user.getEmail())) {
            errors.rejectValue("email", "duplicate.email", "Email already exists");
        }
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of data validation techniques in Spring Data REST and your ability to ensure data integrity.

## ⚠️ Common Mistake
Not validating data on both client and server sides. Another mistake is not handling validation errors properly in the response.

## ⚡ Follow-up Questions

1. How do you customize validation error responses?
2. What is the difference between Bean Validation and custom validators?
3. How do you implement conditional validation?

## 📚 Related Topics

- [Event Handling](./07-event-handling.md)
- [Customizing Endpoints](./04-customizing-endpoints.md)
- [Security](./08-security.md)

## Navigation

[← Previous Question](./08-security.md) | [Next Question →](./10-json-customization.md)
