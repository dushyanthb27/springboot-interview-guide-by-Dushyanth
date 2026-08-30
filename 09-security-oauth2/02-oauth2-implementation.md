# How do you implement OAuth2 with Spring Security? 🟡

## Question

How do you implement OAuth2 authentication and authorization in Spring Boot applications?

## Answer

Spring Security provides comprehensive OAuth2 support through spring-boot-starter-oauth2-resource-server and spring-boot-starter-oauth2-client. Implementation involves configuring the OAuth2 resource server for token validation, setting up security filter chains, and defining access rules. Spring Security handles token validation, user authentication, and authorization automatically.

Configuration includes setting up the JWT decoder, configuring security filter chains, defining access rules based on roles, and integrating with OAuth2 providers like Keycloak or Auth0.

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

The interviewer wants to test your understanding of Spring Security OAuth2 configuration and your ability to secure applications.

## ⚠️ Common Mistake
Not properly configuring the JWT decoder, leading to token validation failures.

## ⚡ Follow-up Questions

1. How do you configure OAuth2 client credentials flow?
2. What is the difference between resource server and client configuration?
3. How do you customize token validation?

## 📚 Related Topics

- [OAuth2 Grant Types](./03-oauth2-grant-types.md)
- [JWT Validation](./04-jwt-validation.md)
- [Method-Level Security](./10-method-level-security.md)

## Navigation

[← Previous Question](./01-what-is-oauth2.md) | [Next Question →](./03-oauth2-grant-types.md)
