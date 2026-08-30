# How does Spring Cloud Gateway work? 🟡

## Question

Explain the architecture and key components of Spring Cloud Gateway.

## Answer

Spring Cloud Gateway is built on Spring Framework 5, Spring Boot 2, and Project Reactor, providing a non-blocking API for routing and filtering requests. It uses three core components: Route (predicate + filter), Predicate (matching conditions for requests), and Filter (modifying requests/responses). It operates on a non-blocking, reactive model using WebFlux and Netty.

The gateway processes requests through a filter chain, where each filter can modify the request or response before passing to the next filter. Routes are matched based on predicates, and filters are applied in pre and post phases. This architecture provides high performance and scalability.

## Code Example

```java
@Configuration
public class GatewayConfig {
    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
        return builder.routes()
                .route("user-service", r -> r
                        .path("/api/users/**")
                        .and()
                        .method(HttpMethod.GET, HttpMethod.POST)
                        .filters(f -> f.filter(new LoggingFilter()))
                        .uri("lb://user-service"))
                .build();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Spring Cloud Gateway's reactive architecture and core components.

## ⚠️ Common Mistake
Not understanding that Spring Cloud Gateway uses WebFlux and is non-blocking, which differs from traditional servlet-based gateways.

## ⚡ Follow-up Questions

1. What is the difference between Spring Cloud Gateway and Zuul?
2. How does the reactive model improve performance?
3. What are the different types of predicates available?

## 📚 Related Topics

- [Routing Configuration](./03-routing-configuration.md)
- [Gateway Filters](./04-gateway-filters.md)
- [Load Balancing](./08-load-balancing.md)

## Navigation

[← Previous Question](./01-what-is-api-gateway.md) | [Next Question →](./03-routing-configuration.md)
