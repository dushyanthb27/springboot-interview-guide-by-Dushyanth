# How do you implement JWT token validation? 🟡

## Question

How do you implement JWT token validation in Spring Security?

## Answer

JWT token validation in Spring Security is configured using JwtDecoder, which validates the token signature, expiration, and claims. Spring Security provides NimbusJwtDecoder for JWT validation with JWK (JSON Web Key) sets. The decoder can be configured with the issuer URI, JWK set URI, or secret key for HMAC validation.

Configuration includes setting up the JwtDecoder bean, configuring the OAuth2 resource server to use JWT, and optionally customizing claim extraction for role-based access control.

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
                .anyRequest().authenticated()
            )
            .oauth2ResourceServer(oauth2 -> oauth2
                .jwt(jwt -> jwt
                    .decoder(jwtDecoder())
                    .jwtAuthenticationConverter(jwtAuthenticationConverter())
                )
            );
        return http.build();
    }

    @Bean
    public JwtDecoder jwtDecoder() {
        return NimbusJwtDecoder.withJwkSetUri("http://auth-server/.well-known/jwks.json")
                .build();
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
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of JWT validation and your ability to configure Spring Security for token-based authentication.

## ⚠️ Common Mistake
Not validating the token signature or expiration, leading to security vulnerabilities.

## ⚡ Follow-up Questions

1. What is the difference between JWK and secret key validation?
2. How do you extract custom claims from JWT?
3. How do you implement token revocation?

## 📚 Related Topics

- [OAuth2 Implementation](./02-oauth2-implementation.md)
- [Token Refresh](./08-token-refresh.md)
- [Method-Level Security](./10-method-level-security.md)

## Navigation

[← Previous Question](./03-oauth2-grant-types.md) | [Next Question →](./05-rbac.md)
