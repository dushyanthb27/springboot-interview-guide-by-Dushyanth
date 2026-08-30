# How do you encrypt sensitive configuration data? 🟠

## Question

How do you encrypt and decrypt sensitive configuration data in Spring Cloud Config?

## Answer

Spring Cloud Config supports encryption of sensitive values using symmetric and asymmetric encryption. The Config Server can encrypt values using a key stored in environment variables or a keystore. Encrypted values are prefixed with {cipher} and decrypted automatically by the Config Client. Encryption can be done using the /encrypt endpoint or command-line tools.

Security approaches include using environment variables for encryption keys, integrating with HashiCorp Vault for secrets management, and using JCEKS keystore for key storage. The encryption/decryption process is transparent to applications using the configuration.

## Code Example

```java
// Enable encryption in Config Server
spring:
  cloud:
    config:
      server:
        encrypt:
          enabled: true
          key: ${ENCRYPTION_KEY}

// Encrypted value in configuration
database:
  password: '{cipher}AQA8...encrypted_value...'

// Using Vault for secrets
spring:
  cloud:
    config:
      server:
        vault:
          host: localhost
          port: 8200
          scheme: http
          authentication: TOKEN
          token: ${VAULT_TOKEN}
          kv:
    enabled: true
    backend: secret
    applicationName: user-service
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of security practices for configuration management and encryption implementation.

## ⚠️ Common Mistake
Storing encryption keys in configuration files instead of environment variables, compromising security.

## ⚡ Follow-up Questions

1. What is the difference between symmetric and asymmetric encryption?
2. How do you rotate encryption keys?
3. When would you use Vault instead of built-in encryption?

## 📚 Related Topics

- [Configuration Sources](./04-configuration-sources.md)
- [Best Practices](./10-best-practices.md)
- [09. Security with OAuth2](../09-security-oauth2/README.md)

## Navigation

[← Previous Question](./06-environment-config.md) | [Next Question →](./08-configuration-versioning.md)
