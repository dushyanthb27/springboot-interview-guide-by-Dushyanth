# How do you handle exceptions in Spring Boot applications? 🟡

## Question

What are the different approaches to exception handling in Spring Boot and what are the best practices?

## Answer

Spring Boot provides multiple exception handling approaches: @ExceptionHandler for controller-specific handling, @ControllerAdvice for global exception handling across controllers, @ResponseStatus for HTTP status mapping, and custom exception classes for business errors. The recommended approach is to use @ControllerAdvice with @ExceptionHandler for global exception handling, combined with custom exception classes and standardized error response structures.

For REST APIs, you should return consistent error responses with appropriate HTTP status codes, error messages, and optionally error codes for client handling. Spring Boot also provides automatic exception handling for common errors like 404 (NoHandlerFoundException) and 500 (internal server errors).

## Code Example

```java
package com.example.demo.exception;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.context.request.WebRequest;
import org.springframework.web.servlet.mvc.method.annotation.ResponseEntityExceptionHandler;

// Custom exception classes
public class ResourceNotFoundException extends RuntimeException {
    private final String resourceId;

    public ResourceNotFoundException(String resourceId) {
        super("Resource not found: " + resourceId);
        this.resourceId = resourceId;
    }

    public String getResourceId() {
        return resourceId;
    }
}

public class BusinessException extends RuntimeException {
    private final String errorCode;

    public BusinessException(String errorCode, String message) {
        super(message);
        this.errorCode = errorCode;
    }

    public String getErrorCode() {
        return errorCode;
    }
}

// Standardized error response
public class ErrorResponse {
    private final int status;
    private final String error;
    private final String message;
    private final String errorCode;
    private final long timestamp;

    public ErrorResponse(int status, String error, String message, String errorCode) {
        this.status = status;
        this.error = error;
        this.message = message;
        this.errorCode = errorCode;
        this.timestamp = System.currentTimeMillis();
    }

    // Getters
    public int getStatus() { return status; }
    public String getError() { return error; }
    public String getMessage() { return message; }
    public String getErrorCode() { return errorCode; }
    public long getTimestamp() { return timestamp; }
}

// Global exception handler
@ControllerAdvice
public class GlobalExceptionHandler extends ResponseEntityExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleResourceNotFound(
            ResourceNotFoundException ex, WebRequest request) {
        ErrorResponse error = new ErrorResponse(
            HttpStatus.NOT_FOUND.value(),
            "Not Found",
            ex.getMessage(),
            "RESOURCE_NOT_FOUND"
        );
        return new ResponseEntity<>(error, HttpStatus.NOT_FOUND);
    }

    @ExceptionHandler(BusinessException.class)
    public ResponseEntity<ErrorResponse> handleBusinessException(
            BusinessException ex, WebRequest request) {
        ErrorResponse error = new ErrorResponse(
            HttpStatus.BAD_REQUEST.value(),
            "Bad Request",
            ex.getMessage(),
            ex.getErrorCode()
        );
        return new ResponseEntity<>(error, HttpStatus.BAD_REQUEST);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGlobalException(
            Exception ex, WebRequest request) {
        ErrorResponse error = new ErrorResponse(
            HttpStatus.INTERNAL_SERVER_ERROR.value(),
            "Internal Server Error",
            "An unexpected error occurred",
            "INTERNAL_ERROR"
        );
        return new ResponseEntity<>(error, HttpStatus.INTERNAL_SERVER_ERROR);
    }
}

// Controller with exception handling
@RestController
@RequestMapping("/api/users")
public class UserController {
    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        User user = userService.getUserById(id)
                .orElseThrow(() -> new ResourceNotFoundException(id.toString()));
        return ResponseEntity.ok(user);
    }

    @PostMapping
    public ResponseEntity<User> createUser(@Valid @RequestBody UserDTO userDTO) {
        if (userDTO.getEmail() == null) {
            throw new BusinessException("INVALID_EMAIL", "Email is required");
        }
        User user = userService.createUser(userDTO);
        return ResponseEntity.status(HttpStatus.CREATED).body(user);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of exception handling patterns and your ability to design a consistent error handling strategy. They're looking for knowledge of global vs local exception handling and proper HTTP status code usage.

## ⚠️ Common Mistake

Not using global exception handling, leading to inconsistent error responses across controllers. Another mistake is returning stack traces in production responses, which can expose sensitive information.

## ⚡ Follow-up Questions

1. How do you handle validation errors in Spring Boot?
2. What is the difference between @ResponseStatus and @ExceptionHandler?
3. How do you customize Spring Boot's default error pages?

## 📚 Related Topics

- [Application Lifecycle](./08-application-lifecycle.md)
- [Custom Annotations](./09-custom-annotations.md)
- [09. Security with OAuth2](../09-security-oauth2/README.md)

## Navigation

[← Previous Question](./10-profiles.md) | [Next Question →](./12-springboot-application.md)
