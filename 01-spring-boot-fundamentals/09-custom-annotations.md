# How do you create and use custom Spring Boot annotations? 🟠

## Question

Explain how to create custom annotations in Spring Boot and provide examples of when custom annotations are useful.

## Answer

Custom annotations in Spring Boot are created using Java's @interface syntax combined with Spring's meta-annotations like @Component, @Configuration, or other stereotype annotations. Custom annotations can be used to simplify repetitive code, implement cross-cutting concerns, or provide domain-specific semantics. They can include retention policies (RUNTIME, CLASS, SOURCE), target types (TYPE, METHOD, FIELD), and can be composed with other annotations using @AliasFor.

Common use cases include custom validation annotations, aspect-oriented programming annotations, conditional bean creation, and domain-specific stereotypes. Custom annotations work with Spring's annotation processing and can be used with @ComponentScan for automatic discovery.

## Code Example

```java
package com.example.demo.annotation;

import org.springframework.stereotype.Component;
import java.lang.annotation.*;

// Custom stereotype annotation
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Repository {
    String value() default "";
}

// Custom validation annotation
@Target({ElementType.FIELD, ElementType.PARAMETER})
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = EmailValidator.class)
public @interface ValidEmail {
    String message() default "Invalid email format";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}

// Custom conditional annotation
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@ConditionalOnProperty(name = "feature.enabled", havingValue = "true")
public @interface EnableFeature {
    String value() default "";
}

// Custom aspect annotation
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface LogExecution {
    String value() default "";
    boolean logArgs() default true;
    boolean logResult() default true;
}

// Usage examples
@Repository("userRepository")
public class UserRepository {
    // Repository implementation
}

public class UserService {
    public void createUser(@ValidEmail String email) {
        // Email will be validated
    }

    @EnableFeature("advanced-search")
    public void advancedSearch() {
        // Only available if feature is enabled
    }

    @LogExecution(logArgs = true, logResult = true)
    public User findUser(String id) {
        // Method execution will be logged
        return userRepository.findById(id);
    }
}

// Email validator implementation
public class EmailValidator implements ConstraintValidator<ValidEmail, String> {
    @Override
    public boolean isValid(String email, ConstraintValidatorContext context) {
        return email != null && email.contains("@");
    }
}

// Aspect for @LogExecution
@Aspect
@Component
public class LoggingAspect {
    @Around("@annotation(logExecution)")
    public Object logExecution(ProceedingJoinPoint joinPoint, LogExecution logExecution) throws Throwable {
        String methodName = joinPoint.getSignature().getName();
        Object[] args = joinPoint.getArgs();

        if (logExecution.logArgs()) {
            System.out.println("Method " + methodName + " called with args: " + Arrays.toString(args));
        }

        Object result = joinPoint.proceed();

        if (logExecution.logResult()) {
            System.out.println("Method " + methodName + " returned: " + result);
        }

        return result;
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of annotation creation and your ability to identify when custom annotations add value. They're looking for awareness of meta-annotations, annotation composition, and practical use cases.

## ⚠️ Common Mistake

Creating custom annotations without understanding the retention policy and target types, leading to annotations not being processed correctly. Another mistake is not using proper meta-annotations, causing the custom annotation to not function as intended.

## ⚡ Follow-up Questions

1. What is the difference between @Inherited and non-inherited annotations?
2. How do you create a custom validation annotation?
3. What are the benefits of using custom annotations over regular methods?

## 📚 Related Topics

- [Stereotype Annotations](./03-stereotype-annotations.md)
- [Conditional Beans](./15-conditional-beans.md)
- [Bean Configuration](./13-bean-configuration.md)

## Navigation

[← Previous Question](./08-application-lifecycle.md) | [Next Question →](./10-profiles.md)
