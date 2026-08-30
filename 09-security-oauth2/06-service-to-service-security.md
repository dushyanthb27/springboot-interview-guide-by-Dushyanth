# How do you secure microservice-to-microservice communication? 🟠

## Question

How do you implement secure communication between microservices using OAuth2?

## Answer

Microservice-to-microservice security is typically implemented using OAuth2 client credentials flow, where each service obtains an access token to authenticate with other services. This involves configuring OAuth2 client credentials for each service, implementing token acquisition and refresh, and validating tokens on the receiving service.

Implementation includes setting up OAuth2 client configuration, using WebClient or RestTemplate with OAuth2 support, and configuring the receiving service as an OAuth2 resource server to validate tokens.

## Code Example

```java
// Service A - OAuth2 Client Configuration
@Configuration
public class OAuth2ClientConfig {
    @Bean
    public WebClient webClient(OAuth2AuthorizedClientManager clientManager) {
        ServerOAuth2AuthorizedClientExchangeFilterFunction oauth =
                new ServerOAuth2AuthorizedClientExchangeFilterFunction(clientManager, "service-a");

        return WebClient.builder()
                .baseUrl("http://service-b")
                .filter(oauth)
                .build();
    }
}

// Service B - OAuth2 Resource Server Configuration
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
                .jwt(jwt -> jwt.jwtDecoder(jwtDecoder()))
            );
        return http.build();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of service-to-service security and your ability to implement OAuth2 client credentials flow.

## ⚠️ Common Mistake
Not properly configuring token refresh, leading to authentication failures when tokens expire.

## ⚡ Follow-up Questions

1. How do you handle token rotation?
2. What is the difference between client credentials and JWT?
3. How do you implement mutual TLS for service-to-service communication?

## 📚 Related Topics

- [OAuth2 Grant Types](./03-oauth2-grant-types.md)
- [JWT Validation](./04-jwt-validation.md)
- [08. Spring Rest Client](../08-spring-rest-client/README.md)

## Navigation

[← Previous Question](./05-rbac.md) | [Next Question →](./07-keycloak-integration.md)
