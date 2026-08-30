# What are the different REST client options in Spring? 🟢

## Question

What are the different REST client options available in Spring Boot and when would you use each?

## Answer

Spring Boot provides multiple REST client options: RestTemplate (traditional synchronous client, now in maintenance mode), WebClient (modern reactive client based on Project Reactor), and third-party libraries like Feign (declarative REST client). RestTemplate is simpler but blocking, while WebClient is non-blocking and reactive. Feign provides declarative, interface-based REST clients.

WebClient is the recommended choice for new applications due to its reactive nature and better performance. RestTemplate can still be used for simple synchronous use cases. Feign is useful for declarative service interfaces in microservices.

## Code Example

```java
// RestTemplate example
@Configuration
public class RestTemplateConfig {
    @Bean
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}

@Service
public class UserServiceClient {
    private final RestTemplate restTemplate;

    public User getUser(Long userId) {
        return restTemplate.getForObject("http://user-service/api/users/" + userId, User.class);
    }
}

// WebClient example
@Configuration
public class WebClientConfig {
    @Bean
    public WebClient webClient() {
        return WebClient.builder()
                .baseUrl("http://user-service")
                .build();
    }
}

@Service
public class UserServiceClient {
    private final WebClient webClient;

    public Mono<User> getUser(Long userId) {
        return webClient.get()
                .uri("/api/users/{id}", userId)
                .retrieve()
                .bodyToMono(User.class);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of the different REST client options and your ability to choose the right one for different scenarios.

## ⚡ Follow-up Questions

1. Why is RestTemplate in maintenance mode?
2. What are the advantages of reactive programming?
3. When would you choose Feign over WebClient?

## 📚 Related Topics

- [WebClient vs RestTemplate](./02-webclient-vs-resttemplate.md)
- [WebClient Configuration](./03-webclient-configuration.md)
- [02. Microservices Architecture](../02-microservices-architecture/README.md)

## Navigation

[← Main Section](./README.md) | [Next Question →](./02-webclient-vs-resttemplate.md)
