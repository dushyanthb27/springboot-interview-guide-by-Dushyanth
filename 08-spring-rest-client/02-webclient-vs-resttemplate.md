# How does WebClient differ from RestTemplate? 🟡

## Question

What are the key differences between WebClient and RestTemplate?

## Answer

WebClient is a reactive, non-blocking HTTP client built on Project Reactor, while RestTemplate is a synchronous, blocking client. WebClient uses less resources and can handle more concurrent connections, making it suitable for microservices with high throughput. RestTemplate is simpler but blocking, which can lead to thread exhaustion under load.

WebClient supports reactive streams (Mono, Flux) for asynchronous processing, while RestTemplate returns blocking results. WebClient is the recommended choice for new applications, while RestTemplate is in maintenance mode and will eventually be removed.

## Code Example

```java
// RestTemplate (blocking)
public User getUser(Long userId) {
    return restTemplate.getForObject("http://user-service/api/users/" + userId, User.class);
}

// WebClient (non-blocking)
public Mono<User> getUser(Long userId) {
    return webClient.get()
            .uri("/api/users/{id}", userId)
            .retrieve()
            .bodyToMono(User.class);
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of synchronous vs reactive programming and the trade-offs between the two approaches.

## ⚠️ Common Mistake
Using RestTemplate in high-throughput scenarios where WebClient would be more efficient.

## ⚡ Follow-up Questions

1. What are Mono and Flux in reactive programming?
2. How does WebClient handle backpressure?
3. What is the migration path from RestTemplate to WebClient?

## 📚 Related Topics

- [WebClient Configuration](./03-webclient-configuration.md)
- [Error Handling](./04-error-handling.md)
- [Connection Pooling](./07-connection-pooling.md)

## Navigation

[← Previous Question](./01-rest-client-options.md) | [Next Question →](./03-webclient-configuration.md)
