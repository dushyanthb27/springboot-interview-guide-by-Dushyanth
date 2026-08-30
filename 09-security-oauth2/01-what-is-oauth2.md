# What is OAuth2 and how does it work? 🟢

## Question

What is OAuth2 and how does the authorization flow work?

## Answer

OAuth2 (Open Authorization 2.0) is an authorization framework that enables applications to obtain limited access to user accounts on an HTTP service. It allows third-party applications to obtain user consent for access without sharing credentials. OAuth2 defines several grant types for different scenarios: authorization code, client credentials, implicit, and resource owner password credentials.

The authorization flow involves four roles: resource owner (user), client (application), authorization server (issues tokens), and resource server (API). The flow typically involves redirecting the user to the authorization server, obtaining consent, receiving an authorization code, exchanging it for an access token, and using the token to access protected resources.

## Code Example

```java
// OAuth2 flow concept
// 1. User requests access
// 2. Application redirects to authorization server
// GET /oauth/authorize?response_type=code&client_id=client_id&redirect_uri=...

// 3. User grants consent
// 4. Authorization server redirects with authorization code
// GET /callback?code=AUTHORIZATION_CODE

// 5. Application exchanges code for access token
POST /oauth/token
{
  "grant_type": "authorization_code",
  "code": "AUTHORIZATION_CODE",
  "redirect_uri": "...",
  "client_id": "client_id",
  "client_secret": "client_secret"
}

// 6. Application uses access token to access resources
GET /api/users
Authorization: Bearer ACCESS_TOKEN
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of OAuth2 fundamentals and the authorization flow.

## ⚡ Follow-up Questions

1. What are the different OAuth2 grant types?
2. What is the difference between OAuth2 and OpenID Connect?
3. What is the purpose of refresh tokens?

## 📚 Related Topics

- [OAuth2 Implementation](./02-oauth2-implementation.md)
- [OAuth2 Grant Types](./03-oauth2-grant-types.md)
- [JWT Validation](./04-jwt-validation.md)

## Navigation

[← Main Section](./README.md) | [Next Question →](./02-oauth2-implementation.md)
