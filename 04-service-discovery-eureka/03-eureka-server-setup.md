# How do you set up a Eureka server? 🟡

## Question

What are the steps to set up a Eureka server in a Spring Boot application?

## Answer

To set up a Eureka server, you need to add the spring-cloud-starter-netflix-eureka-server dependency, annotate your main application class with @EnableEurekaServer, and configure the server properties. The server should not register itself as a client (register-with-eureka=false) and should not fetch registry (fetch-registry=false) to avoid acting as a client.

Key configuration includes setting the server port (default 8761), hostname, and client registration settings. The Eureka server provides a web UI for monitoring registered services and a REST API for service registration and discovery.

## Code Example

```java
// pom.xml dependency
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>

// Main application class
@SpringBootApplication
@EnableEurekaServer
public class EurekaServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }
}

// application.yml
server:
  port: 8761

eureka:
  instance:
    hostname: localhost
  client:
    register-with-eureka: false
    fetch-registry: false
    service-url:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

## 🎯 Interviewer Insight

The interviewer wants to test your ability to set up a Eureka server and understand the key configuration properties needed for proper operation.

## ⚠️ Common Mistake
Forgetting to disable Eureka client behavior on the server, causing the server to try to register with itself.

## ⚡ Follow-up Questions

1. How do you secure the Eureka server?
2. What is the purpose of the Eureka dashboard?
3. How do you configure Eureka server clustering?

## 📚 Related Topics

- [Service Registration](./04-service-registration.md)
- [High Availability](./07-high-availability.md)
- [Security](./10-security.md)

## Navigation

[← Previous Question](./02-how-eureka-works.md) | [Next Question →](./04-service-registration.md)
