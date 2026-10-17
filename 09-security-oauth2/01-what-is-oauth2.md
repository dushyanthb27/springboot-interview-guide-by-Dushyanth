# What is OAuth2 and how does it work? 🟢

## Question

What is OAuth2 and how does the authorization flow work? Explain the different grant types and when to use each one.

## Answer

OAuth2 (Open Authorization 2.0) is an authorization framework that enables applications to obtain limited access to user accounts on an HTTP service. It allows third-party applications to obtain user consent for access without sharing credentials. OAuth2 defines several grant types for different scenarios.

**OAuth2 roles:**
- **Resource owner**: The user who owns the data
- **Client**: The application requesting access
- **Authorization server**: Server that authenticates users and issues tokens
- **Resource server**: The API hosting the protected resources

**OAuth2 grant types and when to use them:**

1. **Authorization Code**: Most secure for web applications. User is redirected to authorization server, grants consent, receives an authorization code, which is exchanged for an access token. The code is short-lived and must be exchanged quickly, preventing token interception.

2. **Authorization Code with PKCE**: Enhanced version of authorization code for public clients (mobile apps, SPAs). Uses Proof Key for Code Exchange to prevent authorization code interception attacks.

3. **Client Credentials**: For machine-to-machine communication (no user involved). Client authenticates directly with authorization server using its client ID and secret to obtain an access token. Used for backend services calling other backend services.

4. **Resource Owner Password Credentials**: Legacy flow where user provides credentials directly to the client. **Not recommended** due to security risks. Only used for trusted first-party applications where other flows aren't feasible.

5. **Implicit (Deprecated)**: Originally for SPAs, token returned directly in URL. Deprecated due to security vulnerabilities. Use Authorization Code with PKCE instead.

**Authorization Code flow (most common):**
1. User requests access from client application
2. Client redirects user to authorization server with client ID and redirect URI
3. User authenticates and grants consent
4. Authorization server redirects back to client with authorization code
5. Client exchanges authorization code for access token (and refresh token)
6. Client uses access token to access protected resources
7. When access token expires, client uses refresh token to obtain new access token

## Code Example

```java
// Spring Boot OAuth2 Client Configuration
@Configuration
@EnableOAuth2Client
public class OAuth2ClientConfig {

    @Bean
    public OAuth2RestTemplate oauth2RestTemplate(OAuth2ClientContext oauth2ClientContext) {
        return new OAuth2RestTemplate(resource(), oauth2ClientContext);
    }

    @Bean
    protected OAuth2ProtectedResourceDetails resource() {
        AuthorizationCodeResourceDetails resource = new AuthorizationCodeResourceDetails();
        resource.setAccessTokenUri("https://auth-server/oauth/token");
        resource.setUserAuthorizationUri("https://auth-server/oauth/authorize");
        resource.setClientId("client-id");
        resource.setClientSecret("client-secret");
        resource.setScope(Arrays.asList("read", "write"));
        return resource;
    }
}

// Using OAuth2 to access protected resources
@RestController
public class ApiController {

    @Autowired
    private OAuth2RestTemplate oauth2RestTemplate;

    @GetMapping("/users")
    public ResponseEntity<List<User>> getUsers() {
        // Automatically handles token acquisition and refresh
        ResponseEntity<User[]> response = oauth2RestTemplate.getForEntity(
            "https://api-server/api/users",
            User[].class
        );
        return ResponseEntity.ok(Arrays.asList(response.getBody()));
    }
}

// OAuth2 flow in HTTP (conceptual)
// 1. User requests access
// 2. Application redirects to authorization server
GET /oauth/authorize?response_type=code&client_id=client_id&redirect_uri=https://app/callback&scope=read

// 3. User grants consent
// 4. Authorization server redirects with authorization code
GET https://app/callback?code=AUTHORIZATION_CODE

// 5. Application exchanges code for access token
POST /oauth/token
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code&code=AUTHORIZATION_CODE&redirect_uri=https://app/callback&client_id=client_id&client_secret=client_secret

// Response:
{
  "access_token": "ACCESS_TOKEN",
  "refresh_token": "REFRESH_TOKEN",
  "expires_in": 3600,
  "token_type": "Bearer"
}

// 6. Application uses access token to access resources
GET /api/users
Authorization: Bearer ACCESS_TOKEN
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of OAuth2 fundamentals and the authorization flow. They're looking for knowledge of different grant types and when to use each one. A strong answer demonstrates understanding of security considerations and practical implementation details.

## ⚠️ Common Mistake

Confusing OAuth2 with authentication. OAuth2 is for authorization (delegated access), not authentication (verifying who a user is). For authentication, you should use OpenID Connect (OIDC), which is built on top of OAuth2. Another mistake is using deprecated grant types like implicit or resource owner password credentials without understanding the security implications.

## ⚡ Follow-up Questions

1. What is the difference between OAuth2 and OpenID Connect?
2. What is the purpose of refresh tokens, and how do they improve security?
3. Why is the implicit grant type deprecated, and what should you use instead?
4. How does PKCE (Proof Key for Code Exchange) improve security for public clients?
5. What are the security considerations when storing access tokens in browser-based applications?

## 📚 Related Topics

- [OAuth2 Implementation](./02-oauth2-implementation.md)
- [OAuth2 Grant Types](./03-oauth2-grant-types.md)
- [JWT Validation](./04-jwt-validation.md)

## Navigation

[← Main Section](./README.md) | [Next Question →](./02-oauth2-implementation.md)
