# How do clients discover services using Eureka? 🟡

## Question

How do Spring Boot clients discover and communicate with services registered in Eureka?

## Answer

Clients discover services using the DiscoveryClient interface or Spring Cloud Load Balancer. DiscoveryClient provides methods to get service instances by service ID, while Spring Cloud Load Balancer integrates with WebClient or RestTemplate for automatic load balancing. Clients can also use @LoadBalanced annotation for automatic service discovery and load balancing.

Service discovery can be client-side (clients query Eureka directly) or server-side (using an API gateway). Spring Cloud provides abstraction layers that hide the complexity of service discovery, allowing developers to use service names instead of hardcoded URLs.

## Code Example

```java
// Using DiscoveryClient
@Service
public class OrderService {
    private final DiscoveryClient discoveryClient;
    private final RestTemplate restTemplate;

    public OrderService(DiscoveryClient discoveryClient, RestTemplate restTemplate) {
        this.discoveryClient = discoveryClient;
        this.restTemplate = restTemplate;
    }

    public User getUser(Long userId) {
        List<ServiceInstance> instances = discoveryClient.getInstances("user-service");
        if (instances.isEmpty()) {
            throw new ServiceUnavailableException("User service not available");
        }
        ServiceInstance instance = instances.get(0);
        String url = instance.getUri() + "/api/users/" + userId;
        return restTemplate.getForObject(url, User.class);
    }
}

// Using @LoadBalanced with RestTemplate
@Configuration
public class RestTemplateConfig {
    @Bean
    @LoadBalanced
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}

@Service
public class OrderService {
    private final RestTemplate restTemplate;

    public OrderService(RestTemplate restTemplate) {
        this.restTemplate = restTemplate;
    }

    public User getUser(Long userId) {
        // Uses service name instead of URL
        return restTemplate.getForObject("http://user-service/api/users/" + userId, User.class);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of client-side service discovery patterns and the Spring Cloud abstractions available.

## ⚠️ Common Mistake
Not handling the case when no service instances are available, leading to runtime exceptions.

## ⚡ Follow-up Questions

1. What is the difference between DiscoveryClient and @LoadBalanced?
2. How does Spring Cloud Load Balancer choose instances?
3. How do you implement custom load balancing strategies?

## 📚 Related Topics

- [Load Balancer Integration](./11-load-balancer-integration.md)
- [Discovery Patterns](./06-discovery-patterns.md)
- [08. Spring Rest Client](../08-spring-rest-client/README.md)

## Navigation

[← Previous Question](./04-service-registration.md) | [Next Question →](./06-discovery-patterns.md)
