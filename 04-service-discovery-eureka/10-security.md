# What are the security considerations for Eureka? 🟠

## Question

How do you secure Eureka server and client communication?

## Answer

Eureka security involves securing the Eureka server with authentication and authorization, encrypting communication between clients and server, and protecting service registration information. Security can be implemented using Spring Security with basic authentication, OAuth2, or mutual TLS.

Key security considerations include: securing the Eureka dashboard, protecting service registration endpoints, encrypting service metadata, and implementing proper authentication for service-to-service communication through Eureka.

## Code Example

```java
// Eureka server security configuration
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable())
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/eureka/**").authenticated()
                .anyRequest().permitAll()
            )
            .httpBasic(withDefaults());
        return http.build();
    }
}

// Client configuration with authentication
eureka:
  client:
    service-url:
      defaultZone: http://user:password@localhost:8761/eureka/
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of security concerns in service discovery and your ability to implement proper security measures.

## ⚠️ Common Mistake
Leaving Eureka server unsecured in production environments, exposing service registry information to unauthorized access.

## ⚡ Follow-up Questions

1. How do you implement OAuth2 security for Eureka?
2. What are the risks of unsecured service discovery?
3. How do you secure service-to-service communication through Eureka?

## 📚 Related Topics

- [09. Security with OAuth2](../09-security-oauth2/README.md)
- [Service Registration](./04-service-registration.md)
- [High Availability](./07-high-availability.md)

## Navigation

[← Previous Question](./09-service-down-handling.md) | [Next Question →](./11-load-balancer-integration.md)
