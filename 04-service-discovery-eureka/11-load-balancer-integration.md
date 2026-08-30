# How do you integrate Eureka with Spring Cloud Load Balancer? 🟠

## Question

Explain how to integrate Eureka with Spring Cloud Load Balancer for client-side load balancing.

## Answer

Spring Cloud Load Balancer replaces Netflix Ribbon and integrates with Eureka for service discovery and load balancing. It provides a client-side load balancer that works with WebClient and RestTemplate using the @LoadBalanced annotation. The load balancer queries Eureka for available service instances and distributes requests using configurable load balancing strategies.

Integration involves adding the spring-cloud-starter-loadbalancer dependency, configuring @LoadBalanced beans, and optionally customizing load balancing strategies. Spring Cloud Load Balancer supports round-robin, random, and custom load balancing algorithms.

## Code Example

```java
// Dependencies
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-loadbalancer</artifactId>
</dependency>

// Load balanced RestTemplate
@Configuration
public class RestTemplateConfig {
    @Bean
    @LoadBalanced
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}

// Load balanced WebClient
@Configuration
public class WebClientConfig {
    @Bean
    @LoadBalanced
    public WebClient.Builder webClientBuilder() {
        return WebClient.builder();
    }
}

// Usage in service
@Service
public class OrderService {
    private final RestTemplate restTemplate;
    private final WebClient webClient;

    public OrderService(RestTemplate restTemplate, WebClient webClient) {
        this.restTemplate = restTemplate;
        this.webClient = webClient;
    }

    public User getUserWithRestTemplate(Long userId) {
        return restTemplate.getForObject("http://user-service/api/users/" + userId, User.class);
    }

    public Mono<User> getUserWithWebClient(Long userId) {
        return webClient.get()
                .uri("http://user-service/api/users/" + userId)
                .retrieve()
                .bodyToMono(User.class);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of modern Spring Cloud load balancing and your ability to integrate it with service discovery.

## ⚠️ Common Mistake
Using the deprecated Netflix Ribbon instead of Spring Cloud Load Balancer in new projects.

## ⚡ Follow-up Questions

1. What are the different load balancing strategies available?
2. How do you implement custom load balancing logic?
3. How does Spring Cloud Load Balancer differ from Ribbon?

## 📚 Related Topics

- [Service Discovery Clients](./05-service-discovery-clients.md)
- [08. Spring Rest Client](../08-spring-rest-client/README.md)
- [Discovery Patterns](./06-discovery-patterns.md)

## Navigation

[← Previous Question](./10-security.md) | [Next Question →](./12-alternatives.md)
