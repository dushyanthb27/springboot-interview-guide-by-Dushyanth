# How do you implement role-based access control? 🟡

## Question

How do you implement role-based access control (RBAC) in Spring Security?

## Answer

Role-based access control in Spring Security is implemented using roles assigned to users and configured in security filter chains. Roles can be extracted from JWT claims, database records, or LDAP. Spring Security provides @PreAuthorize, @Secured, and hasRole() methods for role-based authorization.

Implementation includes configuring role extraction from authentication sources, defining access rules in security filter chains, and using method-level security annotations for fine-grained control.

## Code Example

```java
@Configuration
@EnableWebSecurity
@EnableMethodSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable())
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .requestMatchers("/api/users/**").hasAnyRole("ADMIN", "USER")
                .requestMatchers("/api/public/**").permitAll()
                .anyRequest().authenticated()
            )
            .oauth2ResourceServer(oauth2 -> oauth2
                .jwt(jwt -> jwt.jwtAuthenticationConverter(jwtAuthenticationConverter()))
            );
        return http.build();
    }

    @Bean
    public JwtAuthenticationConverter jwtAuthenticationConverter() {
        JwtGrantedAuthoritiesConverter authoritiesConverter = new JwtGrantedAuthoritiesConverter();
        authoritiesConverter.setAuthorityPrefix("ROLE_");
        authoritiesConverter.setAuthoritiesClaimName("roles");

        JwtAuthenticationConverter converter = new JwtAuthenticationConverter();
        converter.setJwtGrantedAuthoritiesConverter(authoritiesConverter);
        return converter;
    }
}

// Method-level security
@Service
public class UserService {
    @PreAuthorize("hasRole('ADMIN')")
    public void deleteUser(Long userId) {
        // Delete user logic
    }

    @PreAuthorize("hasAnyRole('ADMIN', 'USER')")
    public User getUser(Long userId) {
        // Get user logic
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of RBAC implementation and your ability to configure fine-grained authorization.

## ⚠️ Common Mistake
Not properly extracting roles from JWT claims, leading to authorization failures.

## ⚡ Follow-up Questions

1. What is the difference between hasRole and hasAuthority?
2. How do you implement custom role extraction?
3. What is the difference between @PreAuthorize and @Secured?

## 📚 Related Topics

- [Method-Level Security](./10-method-level-security.md)
- [JWT Validation](./04-jwt-validation.md)
- [Securing REST Endpoints](./11-securing-rest-endpoints.md)

## Navigation

[← Previous Question](./04-jwt-validation.md) | [Next Question →](./06-service-to-service-security.md)
