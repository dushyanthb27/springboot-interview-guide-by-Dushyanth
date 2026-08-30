# How do you handle CORS in Spring Security? 🟡

## Question

How do you configure Cross-Origin Resource Sharing (CORS) in Spring Security?

## Answer

CORS configuration in Spring Security allows or restricts cross-origin requests from web browsers. Spring Security provides built-in CORS support through the CorsConfigurationSource bean. Configuration includes defining allowed origins, methods, headers, and credentials.

Implementation involves creating a CorsConfigurationSource bean, configuring allowed origins and methods, and enabling CORS in the security filter chain. Proper CORS configuration is essential for single-page applications (SPAs) that consume APIs from different domains.

## Code Example

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable())
            .cors(cors -> cors.configurationSource(corsConfigurationSource()))
            .authorizeHttpRequests(auth -> auth
                .anyRequest().authenticated()
            )
            .oauth2ResourceServer(oauth2 -> oauth2
                .jwt(jwt -> jwt.jwtDecoder(jwtDecoder()))
            );
        return http.build();
    }

    @Bean
    public CorsConfigurationSource corsConfigurationSource() {
        CorsConfiguration configuration = new CorsConfiguration();
        configuration.setAllowedOrigins(Arrays.asList("http://localhost:3000"));
        configuration.setAllowedMethods(Arrays.asList("GET", "POST", "PUT", "DELETE", "OPTIONS"));
        configuration.setAllowedHeaders(Arrays.asList("*"));
        configuration.setAllowCredentials(true);

        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", configuration);
        return source;
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of CORS and your ability to configure it properly for web applications.

## ⚠️ Common Mistake
Using wildcard origins with credentials, which is not allowed by browsers for security reasons.

## ⚡ Follow-up Questions

1. Why is CORS needed?
2. What is the difference between simple and preflight requests?
3. How do you handle CORS in a microservices architecture?

## 📚 Related Topics

- [Securing REST Endpoints](./11-securing-rest-endpoints.md)
- [OAuth2 Implementation](./02-oauth2-implementation.md)
- [Custom Security Filters](./12-custom-security-filters.md)

## Navigation

[← Previous Question](./08-token-refresh.md) | [Next Question →](./10-method-level-security.md)
