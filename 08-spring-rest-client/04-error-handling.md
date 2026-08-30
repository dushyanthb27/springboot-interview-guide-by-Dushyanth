# How do you implement error handling in WebClient? 🟡

## Question

How do you implement proper error handling in WebClient for HTTP errors and exceptions?

## Answer

WebClient error handling can be implemented using onStatus() for HTTP status codes, onErrorResume() for exception handling, and custom exception handling. WebClient provides a fluent API to handle different error scenarios like 4xx client errors, 5xx server errors, and network exceptions.

Error handling approaches include using onStatus() to handle specific HTTP status codes, onErrorResume() to recover from exceptions, and custom exception classes for domain-specific errors. Proper error handling ensures graceful degradation and meaningful error messages.

## Code Example

```java
public Mono<User> getUser(Long userId) {
    return webClient.get()
            .uri("/api/users/{id}", userId)
            .retrieve()
            .onStatus(HttpStatus::is4xx, response -> Mono.error(new UserNotFoundException(userId)))
            .onStatus(HttpStatus::is5xx, response -> Mono.error(new UserServiceUnavailableException()))
            .bodyToMono(User.class);
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of WebClient error handling and your ability to implement robust error scenarios.

## ⚠️ Common Mistake
Not handling HTTP errors properly, leading to generic exceptions that don't provide useful information.

## ⚡ Follow-up Questions

1. How do you handle 4xx vs 5xx errors differently?
2. What is the difference between onStatus and onErrorResume?
3. How do you implement custom exception classes?

## 📚 Related Topics

- [Retry Logic](./05-retry-logic.md)
- [WebClient Configuration](./03-webclient-configuration.md)
- [07. Resilience Patterns](../07-resilience-patterns/README.md)

## Navigation

[← Previous Question](./03-webclient-configuration.md) | [Next Question →](./05-retry-logic.md)
