# What are profiles in Spring Boot and how do you use them? 🟡

## Question

Explain Spring Boot profiles and how they help manage environment-specific configurations.

## Answer

Spring Boot profiles provide a mechanism to segregate parts of your application configuration and make it available only in certain environments. Profiles allow you to define different configurations for development, testing, and production environments without changing the application code. You can activate profiles using command-line arguments, environment variables, or programmatically.

Profiles work by loading configuration files like application-dev.properties, application-prod.properties, or using @Profile annotations on beans. Spring Boot also supports profile groups to organize related profiles and profile-specific bean creation using @ConditionalOnProfile.

## Code Example

```java
package com.example.demo.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Profile;
import org.springframework.beans.factory.annotation.Value;

@Configuration
public class DataSourceConfig {

    @Bean
    @Profile("dev")
    public DataSource devDataSource() {
        return new EmbeddedDatabaseBuilder()
                .setType(EmbeddedDatabaseType.H2)
                .build();
    }

    @Bean
    @Profile("prod")
    public DataSource prodDataSource(
            @Value("${spring.datasource.url}") String url,
            @Value("${spring.datasource.username}") String username,
            @Value("${spring.datasource.password}") String password) {
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl(url);
        config.setUsername(username);
        config.setPassword(password);
        return new HikariDataSource(config);
    }

    @Bean
    @Profile("!prod")
    public DataSource testDataSource() {
        return new EmbeddedDatabaseBuilder()
                .setType(EmbeddedDatabaseType.H2)
                .build();
    }
}

// Profile-specific service
@Service
@Profile("dev")
public class DevEmailService implements EmailService {
    @Override
    public void sendEmail(String to, String subject, String body) {
        System.out.println("[DEV] Email to " + to + ": " + subject);
    }
}

@Service
@Profile("prod")
public class ProdEmailService implements EmailService {
    private final JavaMailSender mailSender;

    public ProdEmailService(JavaMailSender mailSender) {
        this.mailSender = mailSender;
    }

    @Override
    public void sendEmail(String to, String subject, String body) {
        SimpleMailMessage message = new SimpleMailMessage();
        message.setTo(to);
        message.setSubject(subject);
        message.setText(body);
        mailSender.send(message);
    }
}
```

**application-dev.properties:**
```properties
# Development configuration
spring.datasource.url=jdbc:h2:mem:devdb
spring.datasource.username=sa
spring.datasource.password=
logging.level.root=DEBUG
app.email.enabled=false
```

**application-prod.properties:**
```properties
# Production configuration
spring.datasource.url=jdbc:postgresql://prod-server:5432/mydb
spring.datasource.username=${DB_USERNAME}
spring.datasource.password=${DB_PASSWORD}
logging.level.root=WARN
app.email.enabled=true
```

**Activating profiles:**
```bash
# Command line
java -jar app.jar --spring.profiles.active=prod

# Environment variable
export SPRING_PROFILES_ACTIVE=prod

# application.properties
spring.profiles.active=dev
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of environment-specific configuration management and your ability to structure applications for different deployment environments. They're looking for knowledge of profile activation and best practices.

## ⚠️ Common Mistake

Hardcoding environment-specific values instead of using profiles, leading to configuration management issues. Another mistake is not properly securing sensitive information in production profiles.

## ⚡ Follow-up Questions

1. How do you create profile groups in Spring Boot?
2. What is the default profile in Spring Boot?
3. How do you test profile-specific configurations?

## 📚 Related Topics

- [Application Properties](./06-application-properties.md)
- [Conditional Beans](./15-conditional-beans.md)
- [06. Configuration Management](../06-configuration-management/README.md)

## Navigation

[← Previous Question](./09-custom-annotations.md) | [Next Question →](./11-exception-handling.md)
