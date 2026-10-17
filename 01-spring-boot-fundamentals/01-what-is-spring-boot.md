# What is Spring Boot and why would you use it? 🟢

## Question

You are onboarding a team that still builds Spring MVC applications with manual XML and standalone servers. Why would you choose Spring Boot for a new service, and how would you justify that decision to engineering leadership?

## Answer

Spring Boot is an opinionated framework that simplifies the development of production-ready Spring applications by providing default configurations, reducing boilerplate code, and eliminating the need for manual XML configuration. It's built on top of the Spring Framework and follows convention over configuration principles.

**Key advantages over traditional Spring:**

1. **Auto-configuration**: Automatically configures your application based on jar dependencies you added, eliminating most manual configuration. For example, if you include `spring-boot-starter-web`, Spring Boot automatically configures an embedded Tomcat, Spring MVC, and sensible defaults for web applications.

2. **Starter dependencies**: Curated dependency sets that simplify dependency management. Instead of adding multiple libraries and managing version compatibility, you add a single starter like `spring-boot-starter-data-jpa` which includes Hibernate, Spring Data JPA, and JDBC with compatible versions.

3. **Embedded servers**: Embed Tomcat, Jetty, or Undertow directly in the application, eliminating the need to deploy WAR files to external servers. Applications become executable JARs with `java -jar`.

4. **Production-ready features**: Built-in metrics, health checks, externalized configuration, and application context monitoring via Spring Boot Actuator. In traditional Spring, you'd need to implement these manually.

5. **No XML configuration**: Replaces verbose XML with annotation-based and Java-based configuration, making code more readable and maintainable.

6. **Simplified dependency management**: The parent POM manages dependency versions, so you don't need to specify versions for most Spring-related libraries.

## Code Example

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
@RestController
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

    @GetMapping("/hello")
    public String hello() {
        return "Hello, Spring Boot!";
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to understand if you know the fundamental purpose of Spring Boot and can articulate its benefits over traditional Spring. They're looking for awareness of how Spring Boot simplifies development and the key features that make it popular for microservices. A strong answer demonstrates you understand not just what Spring Boot does, but the specific problems it solves in the development lifecycle.

## ⚠️ Common Mistake

Treating Spring Boot as a replacement for Spring Framework rather than an enhancement. Spring Boot doesn't replace Spring—it builds on it. Another mistake is listing advantages without explaining how they solve real development pain points. For example, mentioning "auto-configuration" without explaining how it eliminates hours of manual XML configuration.

## ⚡ Follow-up Questions

1. How does Spring Boot differ from the traditional Spring Framework? What specific pain points does it solve?
2. What are some disadvantages or trade-offs of using Spring Boot? When might traditional Spring be a better choice?
3. How does Spring Boot support microservices architecture compared to monolithic applications?
4. What happens under the hood when you run a Spring Boot application? Can you explain the startup process?
5. How does Spring Boot manage dependency versions, and when would you need to override a version?

## 📚 Related Topics

- [Spring Boot Starters](./05-spring-boot-starters.md)
- [Auto-configuration](./02-auto-configuration.md)
- [02. Microservices Architecture](../02-microservices-architecture/README.md)

## Navigation

[← Main Section](./README.md) | [Next Question →](./02-auto-configuration.md)
