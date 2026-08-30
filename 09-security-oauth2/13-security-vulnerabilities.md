# What are common security vulnerabilities? 🟠

## Question

What are common security vulnerabilities in microservices and how do you prevent them?

## Answer

Common security vulnerabilities include SQL injection, XSS (Cross-Site Scripting), CSRF (Cross-Site Request Forgery), insecure direct object references, broken authentication, sensitive data exposure, and insufficient logging and monitoring. OWASP Top 10 provides a comprehensive list of common vulnerabilities.

Prevention strategies include input validation, parameterized queries, proper authentication and authorization, encryption of sensitive data, implementing security headers, and regular security audits and penetration testing.

## Code Example

```java
// SQL Injection Prevention
// Bad: String concatenation
String query = "SELECT * FROM users WHERE username = '" + username + "'";

// Good: Parameterized query
@Query("SELECT u FROM User u WHERE u.username = :username")
User findByUsername(@Param("username") String username);

// XSS Prevention
@Configuration
public class SecurityConfig {
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .headers(headers -> headers
                .contentSecurityPolicy(csp -> csp.policyDirectives("default-src 'self'"))
                .xssProtection(xss -> xss.enable())
            );
        return http.build();
    }
}

// CSRF Protection (for stateful apps)
@Configuration
public class SecurityConfig {
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse()));
        return http.build();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your awareness of common security vulnerabilities and your ability to implement preventive measures.

## ⚠️ Common Mistake
Not implementing proper input validation, leading to injection vulnerabilities.

## ⚡ Follow-up Questions

1. What is the OWASP Top 10?
2. How do you prevent SQL injection?
3. What is the difference between XSS and CSRF?

## 📚 Related Topics

- [Security Best Practices](./14-security-best-practices.md)
- [OAuth2 Implementation](./02-oauth2-implementation.md)
- [Securing REST Endpoints](./11-securing-rest-endpoints.md)

## Navigation

[← Previous Question](./12-custom-security-filters.md) | [Next Question →](./14-security-best-practices.md)
