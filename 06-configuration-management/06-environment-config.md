# How do you handle environment-specific configurations? 🟡

## Question

How do you manage different configurations for different environments (dev, test, prod) in Spring Cloud Config?

## Answer

Environment-specific configurations are managed using Spring profiles and naming conventions. Configuration files are named using the pattern {application}-{profile}.yml or {application}-{profile}.properties. The Config Server serves the appropriate configuration based on the active profile. Multiple profiles can be combined using comma-separated values.

Configuration structure includes base configuration (application.yml) for common settings and profile-specific files for environment-specific overrides. The Config Server can also use directory structures to organize configurations by environment.

## Code Example

```java
// Config repository structure
config-repo/
├── application.yml           # Common configuration
├── application-dev.yml       # Development specific
├── application-test.yml      # Test specific
├── application-prod.yml      # Production specific
├── user-service.yml          # User service common
├── user-service-dev.yml      # User service dev
└── user-service-prod.yml     # User service prod

// Client configuration
spring:
  application:
    name: user-service
  profiles:
    active: prod
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of profile-based configuration management and naming conventions.

## ⚠️ Common Mistake
Not following the correct naming convention for profile-specific files, leading to configuration not being loaded.

## ⚡ Follow-up Questions

1. How do you handle multiple active profiles?
2. What is the configuration loading order?
3. How do you share common configuration across profiles?

## 📚 Related Topics

- [Config Server Setup](./02-config-server-setup.md)
- [Configuration Refresh](./05-configuration-refresh.md)
- [Best Practices](./10-best-practices.md)

## Navigation

[← Previous Question](./05-configuration-refresh.md) | [Next Question →](./07-encryption.md)
