# How do you configure high availability for Eureka? 🟠

## Question

What are the strategies for making Eureka server highly available?

## Answer

Eureka server high availability is achieved by running multiple Eureka server instances that register with each other (peer-to-peer communication). Each Eureka server is configured to register with other Eureka servers, creating a cluster. If one server fails, others continue to operate, maintaining service registry availability.

Configuration involves setting up multiple Eureka server instances, configuring them to register with each other using the serviceUrl property, and ensuring clients can reach at least one Eureka server. DNS-based discovery or load balancers can be used to distribute client requests across Eureka servers.

## Code Example

```java
// Eureka Server 1 configuration
server:
  port: 8761

eureka:
  instance:
    hostname: eureka-server1
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://eureka-server2:8762/eureka/,http://eureka-server3:8763/eureka/

// Eureka Server 2 configuration
server:
  port: 8762

eureka:
  instance:
    hostname: eureka-server2
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://eureka-server1:8761/eureka/,http://eureka-server3:8763/eureka/

// Client configuration with multiple Eureka servers
eureka:
  client:
    service-url:
      defaultZone: http://eureka-server1:8761/eureka/,http://eureka-server2:8762/eureka/,http://eureka-server3:8763/eureka/
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of high availability patterns for service registries and your ability to configure Eureka clustering.

## ⚠️ Common Mistake
Not configuring Eureka servers to register with each other properly, leading to split-brain scenarios where servers don't share registry information.

## ⚡ Follow-up Questions

1. How does Eureka handle network partitions in a cluster?
2. What is the recommended number of Eureka server instances?
3. How do you monitor Eureka server health?

## 📚 Related Topics

- [Eureka Server Setup](./03-eureka-server-setup.md)
- [Health Checks](./08-health-checks.md)
- [Service Down Handling](./09-service-down-handling.md)

## Navigation

[← Previous Question](./06-discovery-patterns.md) | [Next Question →](./08-health-checks.md)
