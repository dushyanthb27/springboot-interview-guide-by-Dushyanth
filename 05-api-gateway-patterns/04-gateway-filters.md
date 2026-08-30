# What are the different types of gateway filters? 🟡

## Question

Explain the different types of filters in Spring Cloud Gateway and when to use each type.

## Answer

Spring Cloud Gateway filters are categorized into Gateway Filters (applied to specific routes) and Global Filters (applied to all routes). Filters can be pre-filters (executed before routing) or post-filters (executed after routing). They can modify requests, responses, or perform cross-cutting concerns like authentication, logging, and rate limiting.

Filter types include GatewayFilter (route-specific), GlobalFilter (applied to all routes), and combinations using GatewayFilterAdapter. Filters can modify the request path, add/remove headers, implement circuit breaking, and perform custom logic during request processing.

## Code Example

```java
// Custom global filter
@Component
public class LoggingGlobalFilter implements GlobalFilter {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        ServerHttpRequest request = exchange.getRequest();
        System.out.println("Request: " + request.getMethod() + " " + request.getURI());
        return chain.filter(exchange);
    }
}

// Custom gateway filter
@Component
public class AuthGatewayFilterFactory extends AbstractGatewayFilterFactory<AuthGatewayFilterFactory.Config> {
    public AuthGatewayFilterFactory() {
        super(Config.class);
    }

    @Override
    public GatewayFilter apply(Config config) {
        return (exchange, chain) -> {
            String token = exchange.getRequest().getHeaders().getFirst("Authorization");
            if (token == null || !isValidToken(token)) {
                exchange.getResponse().setStatusCode(HttpStatus.UNAUTHORIZED);
                return exchange.getResponse().setComplete();
            }
            return chain.filter(exchange);
        };
    }

    private boolean isValidToken(String token) {
        return token != null && token.startsWith("Bearer ");
    }

    public static class Config {
        // Configuration properties
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of filter types and your ability to implement custom filters for cross-cutting concerns.

## ⚠️ Common Mistake
Not understanding the difference between pre and post filters, leading to incorrect filter ordering.

## ⚡ Follow-up Questions

1. How do you control filter execution order?
2. What is the difference between GatewayFilter and GlobalFilter?
3. How do you modify request headers in a filter?

## 📚 Related Topics

- [Routing Configuration](./03-routing-configuration.md)
- [Authentication](./07-authentication.md)
- [Rate Limiting](./05-rate-limiting.md)

## Navigation

[← Previous Question](./03-routing-configuration.md) | [Next Question →](./05-rate-limiting.md)
