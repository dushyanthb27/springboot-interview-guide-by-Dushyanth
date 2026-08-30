# How do you secure REST endpoints? 🟡

## Question

How do you secure REST endpoints in Spring Boot applications using Spring Security?

## Answer

REST endpoint security in Spring Security is configured through security filter chains, where you define access rules for different URL patterns. Security can be applied at the URL level using request matchers, or at the method level using annotations. Spring Security supports various authentication mechanisms including JWT, OAuth2, and basic authentication.

Configuration includes defining URL patterns, specifying access rules (permitAll, authenticated, hasRole), and configuring the authentication mechanism. REST endpoints should follow the principle of least privilege and be secured appropriately based on sensitivity.

## Code Example

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable())
            .authorizeHttpRequests(auth -> auth
                // Public endpoints
                .requestMatchers(HttpMethod.POST, "/api/auth/login").permitAll()
                .requestMatchers(HttpMethod.POST, "/api/auth/register").permitAll()
                .requestMatchers("/api/public/**").permitAll()
                
                // Admin endpoints
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                
                // User endpoints
                .requestMatchers("/api/users/**").hasAnyRole("ADMIN", "USER")
                
                // All other endpoints require authentication
                .anyRequest().authenticated()
            )
            .oauth2ResourceServer(oauth2 -> oauth2
                .jwt(jwt -> jwt.jwtDecoder(jwtDecoder()))
            );
        return http.build();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of REST endpoint security and your ability to configure appropriate access rules.

## ⚠️ Common Mistake
Not disabling CSRF for stateless REST APIs, which can cause issues with POST/PUT/DELETE requests.

## ⚡ Follow-up Questions

1. Why should CSRF be disabled for REST APIs?
2. How do you secure different HTTP methods differently?
3. What is the principle of least privilege?

## 📚 Related Topics

- [Method-Level Security](./10-method-level-security.md)
- [OAuth2 Implementation](./02-oauth2-implementation.md)
- [CORS Handling](./09-cors-handling.md)

## Navigation

[← Previous Question](./10-method-level-security.md) | [Next Question →](./12-custom-security-filters.md)
