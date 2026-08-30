# How do you implement service-to-service authentication in microservices? 🟠

## Question

What are the approaches for implementing authentication and authorization between microservices?

## Answer

Service-to-service authentication approaches include OAuth2 with JWT tokens (services obtain tokens from authorization server and include them in requests), mutual TLS (mTLS) where services authenticate using certificates), API keys (simple but less secure), and service mesh authentication (like Istio's mTLS). OAuth2 with JWT is most common, using client credentials flow for service authentication.

The recommended approach is OAuth2 with JWT where each service gets a client ID/secret, obtains an access token from the authorization server, and includes the token in the Authorization header when calling other services. Resource servers validate the token's signature and claims. For enhanced security, mutual TLS can be combined with OAuth2.

## Code Example

```java
// OAuth2 Client Configuration
@Configuration
public class OAuth2ClientConfig {

    @Bean
    public OAuth2RestTemplate oauth2RestTemplate(OAuth2ClientContext context) {
        return new OAuth2RestTemplate(resource(), context);
    }

    @Bean
    protected OAuth2ProtectedResourceDetails resource() {
        AuthorizationCodeResourceDetails resource = new AuthorizationCodeResourceDetails();
        resource.setClientId("order-service");
        resource.setClientSecret("order-secret");
        resource.setAccessTokenUri("http://auth-server/oauth/token");
        resource.setGrantType("client_credentials");
        return resource;
    }
}

// Resource Server Configuration
@Configuration
@EnableWebSecurity
public class ResourceServerConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/public/**").permitAll()
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

// Service-to-Service Call with OAuth2
@Service
public class UserServiceClient {
    private final OAuth2RestTemplate restTemplate;

    public User getUser(Long userId) {
        return restTemplate.getForObject(
                "http://user-service/api/users/" + userId,
                User.class
        );
    }
}

// JWT Token Validation
@Component
public class JwtTokenValidator {

    private final JwtDecoder jwtDecoder;

    public boolean validateToken(String token) {
        try {
            Jwt jwt = jwtDecoder.decode(token);
            return jwt.getExpiresAt().isAfter(Instant.now());
        } catch (Exception e) {
            return false;
        }
    }

    public String extractServiceId(String token) {
        Jwt jwt = jwtDecoder.decode(token);
        return jwt.getClaimAsString("client_id");
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of security patterns in distributed systems and your familiarity with OAuth2/JWT implementation. They're looking for awareness of security best practices for service-to-service communication.

## ⚠️ Common Mistake
Using basic authentication or API keys for service-to-service communication, which are less secure. Another mistake is not properly validating JWT tokens or not handling token expiration.

## ⚡ Follow-up Questions

1. What is the difference between client credentials flow and authorization code flow?
2. How do you handle token rotation and refresh in microservices?
3. What are the advantages of mutual TLS over OAuth2?

## 📚 Related Topics

- [09. Security with OAuth2](../09-security-oauth2/README.md)
- [Cross-Cutting Concerns](./09-cross-cutting-concerns.md)
- [Communication Patterns](./04-communication-patterns.md)

## Navigation

[← Previous Question](./10-event-driven-architecture.md) | [Next Question →](./12-anti-patterns.md)
