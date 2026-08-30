# How do you use WebClient with OAuth2? 🟠

## Question

How do you implement OAuth2 authentication in WebClient for secure service-to-service communication?

## Answer

WebClient can be configured with OAuth2 using Spring Security's OAuth2 client support. This involves configuring OAuth2 client credentials, token providers, and adding authorization headers to requests. WebClient can automatically obtain and refresh OAuth2 tokens.

Configuration includes setting up OAuth2 client registration, configuring token providers, and using ServerOAuth2AuthorizedClient for WebClient. The client automatically handles token acquisition and refresh.

## Code Example

```java
@Configuration
public class WebClientConfig {
    @Bean
    public WebClient webClient(OAuth2AuthorizedClientManager clientManager) {
        ServerOAuth2AuthorizedClientExchangeFilterFunction oauth =
                new ServerOAuth2AuthorizedClientExchangeFilterFunction(clientManager, "user-service");

        return WebClient.builder()
                .baseUrl("http://user-service")
                .filter(oauth)
                .build();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of OAuth2 integration with WebClient and your ability to implement secure service communication.

## ⚠️ Common Mistake
Not properly configuring token refresh, leading to authentication failures when tokens expire.

## ⚡ Follow-up Questions

1. How does WebClient handle token refresh?
2. What is the difference between client credentials and authorization code flow?
3. How do you configure multiple OAuth2 clients?

## 📚 Related Topics

- [09. Security with OAuth2](../09-security-oauth2/README.md)
- [WebClient Configuration](./03-webclient-configuration.md)
- [Service Authentication](../02-microservices-architecture/11-service-authentication.md)

## Navigation

[← Previous Question](./09-request-logging.md) | [Next Question →](./11-testing-rest-clients.md)
