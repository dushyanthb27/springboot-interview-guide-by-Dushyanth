# How do you use RestTemplate with service discovery? 🟡

## Question

How do you integrate RestTemplate with service discovery for dynamic service resolution?

## Answer

RestTemplate can be integrated with service discovery using @LoadBalanced annotation and Spring Cloud Load Balancer. This enables RestTemplate to resolve service names to actual instance URLs automatically, integrating with Eureka or other service registries. The load balancer distributes requests across available instances.

Configuration includes adding the spring-cloud-starter-loadbalancer dependency and annotating RestTemplate bean with @LoadBalanced. The load balancer queries the service registry and performs client-side load balancing.

## Code Example

```java
@Configuration
public class RestTemplateConfig {
    @Bean
    @LoadBalanced
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}

@Service
public class UserServiceClient {
    private final RestTemplate restTemplate;

    public User getUser(Long userId) {
        // Uses service name instead of URL
        return restTemplate.getForObject("http://user-service/api/users/" + userId, User.class);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of service discovery integration and your ability to use load-balanced REST clients.

## ⚠️ Common Mistake
Not using the @LoadBalanced annotation, preventing service discovery integration.

## ⚡ Follow-up Questions

1. How does the load balancer choose instances?
2. What happens when no instances are available?
3. How do you customize load balancing strategies?

## 📚 Related Topics

- [04. Service Discovery with Eureka](../04-service-discovery-eureka/README.md)
- [Load Balancing](../05-api-gateway-patterns/08-load-balancing.md)
- [WebClient Configuration](./03-webclient-configuration.md)

## Navigation

[← Previous Question](./05-retry-logic.md) | [Next Question →](./07-connection-pooling.md)
