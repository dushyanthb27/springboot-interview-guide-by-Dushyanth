# How do you implement configuration versioning? 🟡

## Question

How do you manage different versions of configuration in Spring Cloud Config?

## Answer

Configuration versioning in Spring Cloud Config is managed through Git branches and tags. The Config Server can serve configuration from different labels (branches, tags, or commit IDs). This allows rolling back to previous configurations or maintaining different versions for different application versions.

Versioning approaches include using Git branches for environment-specific configurations, tags for release versions, and commit IDs for specific configuration snapshots. Clients can specify the label to fetch configuration from a specific version.

## Code Example

```java
// Client configuration with label
spring:
  cloud:
    config:
      uri: http://localhost:8888
      label: v1.0.0  # Use specific tag
      # label: develop  # Use specific branch
      # label: abc123  # Use specific commit

// Config server configuration
spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/username/config-repo
          default-label: main
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of configuration versioning and Git integration.

## ⚠️ Common Mistake
Not using labels properly, leading to clients fetching wrong configuration versions.

## ⚡ Follow-up Questions

1. How do you handle configuration rollback?
2. What is the default label if not specified?
3. How do you manage configuration for different application versions?

## 📚 Related Topics

- [Config Server Setup](./02-config-server-setup.md)
- [Configuration Refresh](./05-configuration-refresh.md)
- [Best Practices](./10-best-practices.md)

## Navigation

[← Previous Question](./07-encryption.md) | [Next Question →](./09-configuration-failures.md)
