# What is an API Gateway and why is it important in microservices? 🟢

## Question

What is an API Gateway and what are its main benefits in microservices architecture?

## Answer

An API Gateway is a server that acts as a single entry point for all client requests to microservices. It handles cross-cutting concerns like routing, load balancing, authentication, rate limiting, caching, and monitoring. Clients communicate with the gateway instead of individual services, simplifying client-side logic and providing a unified interface.

Benefits include simplified client code (clients only need to know the gateway URL), centralized cross-cutting concerns, optimized communication (aggregation of multiple service calls), security enforcement, and monitoring/logging at a single point. It acts as a reverse proxy that routes requests to appropriate backend services.

## Code Example

```java
@SpringBootApplication
public class GatewayApplication {
    public static void main(String[] args) {
        SpringApplication.run(GatewayApplication.class, args);
    }
}

// Simple routing configuration
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
                        .uri("lb://order-service"))
                .build();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of the API Gateway pattern and its role in microservices architecture.

## ⚡ Follow-up Questions

1. What are the disadvantages of using an API Gateway?
2. How does API Gateway differ from service mesh?
3. What cross-cutting concerns can be handled by API Gateway?

## 📚 Related Topics

- [How Spring Cloud Gateway Works](./02-how-spring-cloud-gateway-works.md)
- [Routing Configuration](./03-routing-configuration.md)
- [02. Microservices Architecture](../02-microservices-architecture/README.md)

## Navigation

[← Main Section](./README.md) | [Next Question →](./02-how-spring-cloud-gateway-works.md)
