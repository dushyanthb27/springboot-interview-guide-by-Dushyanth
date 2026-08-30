# How do you configure routing in Spring Cloud Gateway? 🟡

## Question

What are the different ways to configure routing in Spring Cloud Gateway?

## Answer

Spring Cloud Gateway routing can be configured using Java-based configuration (RouteLocator bean), YAML/properties files, or a combination of both. Routes consist of a unique ID, destination URI, predicates (conditions for matching), and filters (modifications to requests/responses). Predicates can match on path, method, header, query parameter, and more.

Configuration approaches include programmatic RouteLocator configuration for complex logic, YAML configuration for simple routing, and dynamic route discovery from service registries. Routes can be configured for load balancing using the lb:// protocol.

## Code Example

```java
// Java-based configuration
@Configuration
public class GatewayConfig {
    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
        return builder.routes()
                .route("user-service", r -> r
                        .path("/api/users/**")
                        .uri("lb://user-service"))
                .route("order-service", r -> r
                        .path("/api/orders/**")
                        .and()
                        .header("X-API-Version", "v1")
                        .uri("lb://order-service"))
                .build();
    }
}

// YAML configuration
spring:
  cloud:
    gateway:
      routes:
        - id: user-service
          uri: lb://user-service
          predicates:
            - Path=/api/users/**
        - id: order-service
          uri: lb://order-service
          predicates:
            - Path=/api/orders/**
            - Header=X-API-Version, v1
```

## 🎯 Interviewer Insight

The interviewer wants to test your ability to configure Spring Cloud Gateway routing using different approaches.

## ⚠️ Common Mistake
Not using the lb:// protocol for load balancing to services registered with Eureka.

## ⚡ Follow-up Questions

1. How do you configure dynamic routing based on service discovery?
2. What are the different types of predicates available?
3. How do you prioritize route matching?

## 📚 Related Topics

- [Gateway Filters](./04-gateway-filters.md)
- [Load Balancing](./08-load-balancing.md)
- [Routing Patterns](./09-routing-patterns.md)

## Navigation

[← Previous Question](./02-how-spring-cloud-gateway-works.md) | [Next Question →](./04-gateway-filters.md)
