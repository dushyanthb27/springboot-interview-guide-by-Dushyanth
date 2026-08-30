# How do you handle cross-cutting concerns in microservices? 🟡

## Question

What are cross-cutting concerns in microservices architecture and what patterns can be used to handle them effectively?

## Answer

Cross-cutting concerns in microservices include logging, monitoring, security, configuration, caching, error handling, and service discovery. Patterns to handle them include API Gateway (centralized entry point for cross-cutting concerns), Service Mesh (dedicated infrastructure layer for service-to-service communication), Sidecar pattern (deploy helper components alongside services), and Shared Libraries (common functionality packaged as libraries).

The API Gateway pattern handles authentication, rate limiting, and request routing centrally. Service Mesh (like Istio) provides service discovery, load balancing, encryption, and observability without changing application code. Sidecar pattern extends service functionality without modifying the service itself. The choice depends on complexity, team expertise, and infrastructure requirements.

## Code Example

```java
// API Gateway Pattern for Cross-Cutting Concerns
@Configuration
public class GatewayConfig {

    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
        return builder.routes()
                .route("user-service", r -> r
                        .path("/api/users/**")
                        .filters(f -> f
                                .filter(new AuthenticationFilter())
                                .filter(new RateLimitFilter())
                                .filter(new LoggingFilter()))
                        .uri("lb://user-service"))
                .build();
    }
}

// Global Filter for Authentication
@Component
public class AuthenticationFilter implements GlobalFilter {

    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        String token = exchange.getRequest().getHeaders().getFirst("Authorization");
        if (token == null || !isValidToken(token)) {
            exchange.getResponse().setStatusCode(HttpStatus.UNAUTHORIZED);
            return exchange.getResponse().setComplete();
        }
        return chain.filter(exchange);
    }

    private boolean isValidToken(String token) {
        // Token validation logic
        return true;
    }
}

// Sidecar Pattern for Logging
@Configuration
public class LoggingConfig {

    @Bean
    public LogstashTcpSocketAppender logstashAppender() {
        LogstashTcpSocketAppender appender = new LogstashTcpSocketAppender();
        appender.setDestination("logstash:5000");
        appender.start();
        return appender;
    }
}

// Centralized Configuration
@Configuration
@ConfigurationProperties(prefix = "app")
public class AppConfig {
    private String name;
    private Map<String, String> services;

    // Getters and setters
}

// Shared Library for Common Functionality
public class ServiceUtils {
    public static String generateTraceId() {
        return UUID.randomUUID().toString();
    }

    public static void logRequest(String service, String method, String path) {
        MDC.put("service", service);
        MDC.put("method", method);
        MDC.put("path", path);
        MDC.put("traceId", generateTraceId());
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of architectural patterns for handling common concerns across multiple services. They're looking for awareness of different approaches and their trade-offs.

## ⚠️ Common Mistake
Duplicating cross-cutting concern logic across multiple services instead of centralizing it. Another mistake is not understanding when to use API Gateway vs Service Mesh vs shared libraries.

## ⚡ Follow-up Questions

1. What is the difference between API Gateway and Service Mesh?
2. How do you implement distributed tracing across microservices?
3. When would you use the sidecar pattern?

## 📚 Related Topics

- [05. API Gateway Patterns](../05-api-gateway-patterns/README.md)
- [Monitoring and Observability](./13-monitoring-observability.md)
- [Service Authentication](./11-service-authentication.md)

## Navigation

[← Previous Question](./08-deployment-patterns.md) | [Next Question →](./10-event-driven-architecture.md)
