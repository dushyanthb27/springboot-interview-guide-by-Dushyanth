# How do you register a service with Eureka? 🟡

## Question

What are the steps to register a Spring Boot service as a Eureka client?

## Answer

To register a service with Eureka, add the spring-cloud-starter-netflix-eureka-client dependency, annotate the main class with @EnableDiscoveryClient, and configure the Eureka server URL in application properties. The service will automatically register with Eureka on startup and send periodic heartbeats to maintain its registration.

Key configuration includes the application name (used as service ID), Eureka server URL, instance hostname/IP, and health check configuration. The service will automatically deregister when it shuts down gracefully.

## Code Example

```java
// pom.xml dependency
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>

// Main application class
@SpringBootApplication
@EnableDiscoveryClient
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}

// application.yml
spring:
  application:
    name: user-service

eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/
  instance:
    prefer-ip-address: true
    health-check-url-path: /actuator/health
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of client-side Eureka configuration and the automatic registration process.

## ⚠️ Common Mistake
Not setting the application name, which is required for service discovery. Another mistake is not configuring the correct Eureka server URL.

## ⚡ Follow-up Questions

1. How does a service deregister from Eureka?
2. What happens if the Eureka server is unavailable during registration?
3. How do you customize the service instance metadata?

## 📚 Related Topics

- [Service Discovery Clients](./05-service-discovery-clients.md)
- [Health Checks](./08-health-checks.md)
- [Eureka Server Setup](./03-eureka-server-setup.md)

## Navigation

[← Previous Question](./03-eureka-server-setup.md) | [Next Question →](./05-service-discovery-clients.md)
