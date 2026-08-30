# How do you implement load balancing in API Gateway? 🟡

## Question

How does Spring Cloud Gateway integrate with service discovery for load balancing?

## Answer

Spring Cloud Gateway integrates with service discovery (Eureka, Consul) to perform client-side load balancing. Using the lb:// protocol in route URIs, the gateway automatically resolves service instances and distributes requests across them. It supports round-robin, random, and custom load balancing strategies through Spring Cloud Load Balancer.

Load balancing can be configured per route or globally. The gateway can also implement health checks to remove unhealthy instances from the load balancer pool and retry requests to other instances if one fails.

## Code Example

```java
// Load balanced routing
@Configuration
public class GatewayConfig {
    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
        return builder.routes()
                .route("user-service", r -> r
                        .path("/api/users/**")
                        .uri("lb://user-service")) // lb:// enables load balancing
                .build();
    }
}

// Custom load balancing configuration
@Configuration
public class LoadBalancerConfig {
    @Bean
    public ServiceInstanceListSupplier serviceInstanceListSupplier(ConfigurableApplicationContext context) {
        return ServiceInstanceListSupplier.builder()
                .withDiscoveryClient()
                .withHealthChecks()
                .build(context);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of load balancing integration and your ability to configure service discovery-based routing.

## ⚠️ Common Mistake
Not using the lb:// protocol, which prevents load balancing and service discovery integration.

## ⚡ Follow-up Questions

1. What load balancing strategies are available?
2. How does the gateway handle unhealthy service instances?
3. How do you implement custom load balancing logic?

## 📚 Related Topics

- [Routing Configuration](./03-routing-configuration.md)
- [Service Discovery](../04-service-discovery-eureka/README.md)
- [Circuit Breaking](./06-circuit-breaking.md)

## Navigation

[← Previous Question](./07-authentication.md) | [Next Question →](./09-routing-patterns.md)
