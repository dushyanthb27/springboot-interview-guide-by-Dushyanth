# What are the different OAuth2 grant types? 🟡

## Question

Explain the different OAuth2 grant types and when to use each one.

## Answer

OAuth2 defines several grant types: Authorization Code (recommended for server-side apps with user interaction), Client Credentials (for service-to-service communication), Implicit (deprecated, for browser-based apps), Resource Owner Password Credentials (for trusted first-party apps), and Refresh Token (for obtaining new access tokens). Each grant type is designed for specific use cases.

Authorization Code is the most secure for user-facing applications. Client Credentials is used for machine-to-machine communication. Implicit is deprecated in favor of Authorization Code with PKCE. Resource Owner Password should only be used for trusted applications.

## Code Example

```java
// Client Credentials Flow (Service-to-Service)
@Configuration
public class ClientCredentialsConfig {
    @Bean
    public OAuth2AuthorizedClientManager authorizedClientManager(
            ClientRegistrationRepository clientRegistrationRepository,
            OAuth2AuthorizedClientRepository authorizedClientRepository) {
        OAuth2AuthorizedClientProvider authorizedClientProvider =
                OAuth2AuthorizedClientProviderBuilder.builder()
                        .clientCredentials()
                        .build();

        DefaultOAuth2AuthorizedClientManager authorizedClientManager =
                new DefaultOAuth2AuthorizedClientManager(
                        clientRegistrationRepository, authorizedClientRepository);
        authorizedClientManager.setAuthorizedClientProvider(authorizedClientProvider);
        return authorizedClientManager;
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of OAuth2 grant types and your ability to choose the right one for different scenarios.

## ⚠️ Common Mistake
Using the Resource Owner Password Credentials flow for third-party applications, which is insecure.

## ⚡ Follow-up Questions

1. Why is the Implicit grant type deprecated?
2. What is PKCE and when should you use it?
3. How do you implement refresh token flow?

## 📚 Related Topics

- [OAuth2 Implementation](./02-oauth2-implementation.md)
- [Token Refresh](./08-token-refresh.md)
- [Service-to-Service Security](./06-service-to-service-security.md)

## Navigation

[← Previous Question](./02-oauth2-implementation.md) | [Next Question →](./04-jwt-validation.md)
