# How do you handle authentication and authorization in API Gateway? 🟠

## Question

How do you implement authentication and authorization in Spring Cloud Gateway?

## Answer

Spring Cloud Gateway can handle authentication and authorization using Spring Security, OAuth2, JWT tokens, or custom authentication mechanisms. The gateway can validate tokens, enforce security policies, and propagate user context to backend services. Authentication can be implemented using filters that validate credentials before routing requests.

Common approaches include JWT token validation, OAuth2 with Keycloak integration, API key authentication, and custom authentication filters. The gateway can also implement role-based access control and propagate security context to downstream services.

## Code Example

```java
@Configuration
@EnableWebFluxSecurity
public class SecurityConfig {

    @Bean
    public SecurityWebFilterChain securityFilterChain(ServerHttpSecurity http) {
        http
            .csrf(csrf -> csrf.disable())
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/public/**").permitAll()
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .oauth2ResourceServer(oauth2 -> oauth2
                .jwt(jwt -> jwt.jwtDecoder(jwtDecoder()))
            );
        return http.build();
    }

    @Bean
    public JwtDecoder jwtDecoder() {
        return NimbusJwtDecoder.withJwkSetUri("http://auth-server/.well-known/jwks.json")
                .build();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of security implementation in API Gateway and your familiarity with Spring Security integration.

## ⚠️ Common Mistake
Not properly propagating security context to backend services, leading to authentication failures downstream.

## ⚡ Follow-up Questions

1. How do you propagate security context to backend services?
2. What is the difference between authentication and authorization?
3. How do you implement OAuth2 in API Gateway?

## 📚 Related Topics

- [09. Security with OAuth2](../09-security-oauth2/README.md)
- [Gateway Filters](./04-gateway-filters.md)
- [Service Authentication](../02-microservices-architecture/11-service-authentication.md)

## Navigation

[← Previous Question](./06-circuit-breaking.md) | [Next Question →](./08-load-balancing.md)
