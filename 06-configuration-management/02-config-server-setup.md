# How do you set up a Config Server? 🟡

## Question

What are the steps to set up a Spring Cloud Config Server?

## Answer

To set up a Config Server, add the spring-cloud-config-server dependency, annotate the main class with @EnableConfigServer, and configure the repository location (Git, SVN, or local file system). The server needs to be configured with the repository URI, search paths, and authentication if using private repositories.

Configuration includes setting the server port, repository details, and security settings. The Config Server provides REST endpoints for accessing configuration by application name, profile, and label (branch/tag).

## Code Example

```java
// pom.xml dependency
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>

// Main application class
@SpringBootApplication
@EnableConfigServer
public class ConfigServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConfigServerApplication.class, args);
    }
}

// application.yml
server:
  port: 8888

spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/username/config-repo
          search-paths: configs
          username: ${GIT_USERNAME}
          password: ${GIT_PASSWORD}
```

## 🎯 Interviewer Insight

The interviewer wants to test your ability to set up a Config Server and understand the key configuration properties.

## ⚠️ Common Mistake
Not properly configuring Git authentication for private repositories, leading to configuration fetch failures.

## ⚡ Follow-up Questions

1. How do you configure multiple Git repositories?
2. What is the purpose of search paths in Config Server?
3. How do you secure the Config Server?

## 📚 Related Topics

- [Config Client Setup](./03-config-client-setup.md)
- [Configuration Sources](./04-configuration-sources.md)
- [Encryption](./07-encryption.md)

## Navigation

[← Previous Question](./01-what-is-spring-cloud-config.md) | [Next Question →](./03-config-client-setup.md)
