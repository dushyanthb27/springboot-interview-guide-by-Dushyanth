# What are the different configuration sources? 🟡

## Question

What are the different storage backends for Spring Cloud Config configuration?

## Answer

Spring Cloud Config supports multiple configuration storage backends including Git (most common), SVN, Vault (for secrets), local file system, JDBC databases, AWS S3, and parameter stores. Git is the most popular due to its version control capabilities and ease of use. Vault is specifically designed for secrets management.

Each backend has specific configuration requirements. Git-based storage uses standard Git repositories with YAML/properties files. Vault integration requires Vault server setup and configuration. Local file system is useful for development and testing.

## Code Example

```java
// Git-based configuration
spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/username/config-repo
          search-paths: configs

// Vault-based configuration
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

// JDBC-based configuration
spring:
  cloud:
    config:
      server:
        jdbc:
          sql: SELECT key, value from properties where application=? and profile=? and label=?
```

## 🎯 Interviewer Insight

The interviewer wants to test your knowledge of different configuration storage options and when to use each.

## ⚠️ Common Mistake
Using Git for storing sensitive information like passwords, which should be stored in Vault or encrypted.

## ⚡ Follow-up Questions

1. When would you choose Vault over Git?
2. How do you configure multiple configuration sources?
3. What are the advantages of using Git for configuration?

## 📚 Related Topics

- [Config Server Setup](./02-config-server-setup.md)
- [Encryption](./07-encryption.md)
- [Best Practices](./10-best-practices.md)

## Navigation

[← Previous Question](./03-config-client-setup.md) | [Next Question →](./05-configuration-refresh.md)
