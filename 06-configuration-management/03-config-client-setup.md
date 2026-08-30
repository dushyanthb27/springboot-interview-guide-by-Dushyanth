# How do you configure a Config Client? 🟡

## Question

What are the steps to configure a Spring Boot application as a Config Client?

## Answer

To configure a Config Client, add the spring-cloud-starter-config dependency, configure the Config Server URL in bootstrap.yml or application.yml, and set the application name and profile. The client will fetch configuration from the Config Server on startup based on application name, profile, and label.

Configuration includes setting spring.cloud.config.uri for the Config Server URL, spring.application.name for the application identifier, and spring.profiles.active for the active profile. The client can also be configured to fail fast if the Config Server is unavailable.

## Code Example

```java
// pom.xml dependency
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>

// bootstrap.yml
spring:
  application:
    name: user-service
  cloud:
    config:
      uri: http://localhost:8888
      fail-fast: true
      retry:
        initial-interval: 1000
        max-attempts: 3

// application.yml
spring:
  profiles:
    active: dev
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of client-side configuration and the bootstrap process.

## ⚠️ Common Mistake
Not setting the application name, which is required for the Config Client to fetch the correct configuration.

## ⚡ Follow-up Questions

1. What is the difference between bootstrap.yml and application.yml?
2. How does the client handle Config Server unavailability?
3. What is the purpose of the label in configuration fetching?

## 📚 Related Topics

- [Config Server Setup](./02-config-server-setup.md)
- [Configuration Refresh](./05-configuration-refresh.md)
- [Environment Config](./06-environment-config.md)

## Navigation

[← Previous Question](./02-config-server-setup.md) | [Next Question →](./04-configuration-sources.md)
