# What are the different ways to secure Spring Data REST endpoints? 🟠

## Question

How do you implement security for Spring Data REST endpoints using Spring Security?

## Answer

Spring Data REST endpoints can be secured using Spring Security through method-level security annotations (@PreAuthorize, @PostAuthorize), URL-based security configuration, or a combination of both. You can secure specific HTTP methods, apply security based on roles or permissions, and implement custom security rules.

Security approaches include configuring HttpSecurity to protect repository endpoints, using @PreAuthorize on repository methods to restrict access based on roles, implementing custom filters for authentication, and securing specific entities or operations. Spring Security integrates seamlessly with Spring Data REST to provide comprehensive security.

## Code Example

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers(HttpMethod.GET, "/api/users/**").hasAnyRole("USER", "ADMIN")
                .requestMatchers(HttpMethod.POST, "/api/users/**").hasRole("ADMIN")
                .requestMatchers(HttpMethod.PUT, "/api/users/**").hasRole("ADMIN")
                .requestMatchers(HttpMethod.DELETE, "/api/users/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .httpBasic(withDefaults());
        return http.build();
    }
}

// Method-level security on repository
@RepositoryRestResource(collectionResourceRel = "users", path = "users")
public interface UserRepository extends JpaRepository<User, Long> {

    @PreAuthorize("hasRole('ADMIN')")
    User findByEmail(String email);

    @PreAuthorize("#id == authentication.principal.id or hasRole('ADMIN')")
    User findById(Long id);
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of securing Spring Data REST endpoints and your familiarity with Spring Security integration.

## ⚠️ Common Mistake
Not securing all HTTP methods properly, leaving endpoints vulnerable. Another mistake is not considering the security implications of HATEOAS links and discoverability.

## ⚡ Follow-up Questions

1. How do you secure specific HTTP methods differently?
2. What is the difference between URL-based and method-level security?
3. How do you implement custom security rules for Spring Data REST?

## 📚 Related Topics

- [09. Security with OAuth2](../09-security-oauth2/README.md)
- [Customizing Endpoints](./04-customizing-endpoints.md)
- [Event Handling](./07-event-handling.md)

## Navigation

[← Previous Question](./07-event-handling.md) | [Next Question →](./09-validation.md)
