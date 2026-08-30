# What are the different service discovery patterns? 🟡

## Question

Explain the different service discovery patterns and their trade-offs.

## Answer

The two main service discovery patterns are client-side discovery (clients query the service registry and load balance requests themselves) and server-side discovery (clients make requests through a load balancer that queries the service registry). Client-side discovery reduces network hops but requires client-side logic, while server-side discovery centralizes load balancing logic but adds an extra network hop.

Other patterns include peer-to-peer discovery (services discover each other without a central registry) and hybrid approaches. The choice depends on requirements like complexity, performance, and operational overhead.

## Code Example

```java
// Client-side discovery pattern
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
        ServiceInstance instance = loadBalance(instances);
        return restTemplate.getForObject(instance.getUri() + "/api/users/" + userId, User.class);
    }

    private ServiceInstance loadBalance(List<ServiceInstance> instances) {
        // Simple round-robin load balancing
        return instances.get(ThreadLocalRandom.current().nextInt(instances.size()));
    }
}

// Server-side discovery pattern (using API Gateway)
// Client makes request to gateway, gateway handles discovery
// Gateway configuration
@Configuration
public class GatewayConfig {
    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
        return builder.routes()
                .route("user-service", r -> r
                        .path("/api/users/**")
                        .uri("lb://user-service")) // lb = load balancer
                .build();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of architectural patterns for service discovery and your ability to choose the right approach.

## ⚠️ Common Mistake
Not understanding the trade-offs between client-side and server-side discovery, leading to inappropriate pattern selection.

## ⚡ Follow-up Questions

1. What are the advantages of client-side discovery?
2. When would you choose server-side discovery?
3. How does Kubernetes service discovery differ from Eureka?

## 📚 Related Topics

- [How Eureka Works](./02-how-eureka-works.md)
- [Service Discovery Clients](./05-service-discovery-clients.md)
- [05. API Gateway Patterns](../05-api-gateway-patterns/README.md)

## Navigation

[← Previous Question](./05-service-discovery-clients.md) | [Next Question →](./07-high-availability.md)
