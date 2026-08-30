# What is Spring Boot and why would you use it? 🟢

## Question

What is Spring Boot and what are the main advantages of using it over traditional Spring Framework applications?

## Answer

Spring Boot is an opinionated framework that simplifies the development of production-ready Spring applications by providing default configurations, reducing boilerplate code, and eliminating the need for manual XML configuration. It's built on top of the Spring Framework and follows convention over configuration principles.

The main advantages include auto-configuration, embedded servers, starter dependencies for simplified dependency management, production-ready features like metrics and health checks, and the ability to create standalone applications without requiring external server deployment.

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

The interviewer wants to understand if you know the fundamental purpose of Spring Boot and can articulate its benefits over traditional Spring. They're looking for awareness of how Spring Boot simplifies development and the key features that make it popular for microservices.

## ⚡ Follow-up Questions

1. How does Spring Boot differ from the traditional Spring Framework?
2. What are some disadvantages of using Spring Boot?
3. How does Spring Boot support microservices architecture?

## 📚 Related Topics

- [Spring Boot Starters](./05-spring-boot-starters.md)
- [Auto-configuration](./02-auto-configuration.md)
- [02. Microservices Architecture](../02-microservices-architecture/README.md)

## Navigation

[← Main Section](./README.md) | [Next Question →](./02-auto-configuration.md)
