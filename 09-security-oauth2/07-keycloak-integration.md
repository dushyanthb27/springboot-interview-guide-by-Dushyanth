# How do you integrate with Keycloak? 🟠

## Question

How do you integrate Spring Boot applications with Keycloak for authentication and authorization?

## Answer

Keycloak integration with Spring Boot is done through Spring Security OAuth2 resource server configuration. Keycloak acts as the OAuth2 authorization server, providing user authentication, token issuance, and role management. Spring Security validates Keycloak-issued JWT tokens and extracts user roles for authorization.

Configuration includes setting up the Keycloak issuer URI, configuring the JWT decoder to use Keycloak's JWK set, and optionally configuring Keycloak admin client for user management.

## Code Example

```java
@Configuration
@EnableWebSecurity
@EnableMethodSecurity
public class KeycloakSecurityConfig {

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
                .jwt(jwt -> jwt
                    .decoder(jwtDecoder())
                    .jwtAuthenticationConverter(jwtAuthenticationConverter())
                )
            );
        return http.build();
    }

    @Bean
    public JwtDecoder jwtDecoder() {
        return NimbusJwtDecoder.withJwkSetUri(
                "http://localhost:8080/realms/myrealm/protocol/openid-connect/certs"
        ).build();
    }

    @Bean
    public JwtAuthenticationConverter jwtAuthenticationConverter() {
        JwtGrantedAuthoritiesConverter authoritiesConverter = new JwtGrantedAuthoritiesConverter();
        authoritiesConverter.setAuthorityPrefix("ROLE_");
        authoritiesConverter.setAuthoritiesClaimName("realm_access.roles");

        JwtAuthenticationConverter converter = new JwtAuthenticationConverter();
        converter.setJwtGrantedAuthoritiesConverter(authoritiesConverter);
        return converter;
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Keycloak integration and your ability to configure Spring Security with an external OAuth2 provider.

## ⚠️ Common Mistake
Not configuring the correct JWK set URI, leading to token validation failures.

## ⚡ Follow-up Questions

1. How do you configure Keycloak client registration?
2. What is the difference between Keycloak and Auth0?
3. How do you implement Keycloak admin client integration?

## 📚 Related Topics

- [OAuth2 Implementation](./02-oauth2-implementation.md)
- [JWT Validation](./04-jwt-validation.md)
- [RBAC](./05-rbac.md)

## Navigation

[← Previous Question](./06-service-to-service-security.md) | [Next Question →](./08-token-refresh.md)
