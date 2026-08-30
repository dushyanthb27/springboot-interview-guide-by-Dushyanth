# How do you configure application properties in Spring Boot? 🟡

## Question

What are the different ways to configure application properties in Spring Boot and how do you access them in your code?

## Answer

Spring Boot provides multiple ways to configure application properties: application.properties or application.yml files, environment variables, command-line arguments, and @ConfigurationProperties classes. Properties can be externalized for different environments using profiles (application-dev.properties, application-prod.properties). You can access properties using @Value annotation, @ConfigurationProperties for type-safe binding, or Environment abstraction.

Spring Boot loads properties from multiple locations in a specific order: command-line arguments, SPRING_APPLICATION_JSON, JNDI attributes, Java system properties, OS environment variables, RandomValuePropertySource, application-{profile}.properties outside packaged jar, application-{profile}.properties inside packaged jar, application.properties outside jar, and application.properties inside jar.

## Code Example

```java
package com.example.demo.config;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Configuration;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

// Type-safe configuration properties
@Configuration
@ConfigurationProperties(prefix = "app")
public class AppProperties {
    private String name;
    private int timeout;
    private Database database;

    // Getters and setters
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getTimeout() { return timeout; }
    public void setTimeout(int timeout) { this.timeout = timeout; }
    public Database getDatabase() { return database; }
    public void setDatabase(Database database) { this.database = database; }

    public static class Database {
        private String url;
        private String username;
        private String password;

        // Getters and setters
        public String getUrl() { return url; }
        public void setUrl(String url) { this.url = url; }
        public String getUsername() { return username; }
        public void setUsername(String username) { this.username = username; }
        public String getPassword() { return password; }
        public void setPassword(String password) { this.password = password; }
    }
}

// Using @Value annotation
@Component
public class EmailService {
    @Value("${email.sender}")
    private String sender;

    @Value("${email.enabled:false}")
    private boolean enabled;

    public void sendEmail(String to, String subject, String body) {
        if (enabled) {
            System.out.println("Sending email from " + sender + " to " + to);
        }
    }
}
```

**application.properties:**
```properties
# Application configuration
app.name=My Application
app.timeout=5000

# Database configuration
app.database.url=jdbc:postgresql://localhost:5432/mydb
app.database.username=postgres
app.database.password=password

# Email configuration
email.sender=noreply@example.com
email.enabled=true
```

**application.yml:**
```yaml
app:
  name: My Application
  timeout: 5000
  database:
    url: jdbc:postgresql://localhost:5432/mydb
    username: postgres
    password: password

email:
  sender: noreply@example.com
  enabled: true
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Spring Boot's configuration management and your ability to work with different configuration approaches. They're looking for knowledge of property loading order and best practices for externalizing configuration.

## ⚠️ Common Mistake

Hardcoding sensitive information like passwords in application.properties instead of using environment variables or secret management. Another mistake is not understanding the property loading order, leading to configuration not being applied as expected.

## ⚡ Follow-up Questions

1. What is the difference between @Value and @ConfigurationProperties?
2. How do you handle environment-specific configurations?
3. How do you encrypt sensitive properties in Spring Boot?

## 📚 Related Topics

- [Profiles](./10-profiles.md)
- [Bean Configuration](./13-bean-configuration.md)
- [06. Configuration Management](../06-configuration-management/README.md)

## Navigation

[← Previous Question](./05-spring-boot-starters.md) | [Next Question →](./07-dependency-injection-methods.md)
