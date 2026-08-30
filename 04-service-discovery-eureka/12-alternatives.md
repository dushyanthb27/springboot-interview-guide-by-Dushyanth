# What are the alternatives to Eureka? 🟡

## Question

What are other service discovery solutions besides Netflix Eureka and when would you use them?

## Answer

Alternatives to Eureka include Consul (HashiCorp), Zookeeper (Apache), etcd (CoreOS), Kubernetes service discovery, and Spring Cloud Kubernetes. Consul provides service discovery, health checking, and KV store with a simpler setup than Eureka. Zookeeper offers strong consistency but is more complex to operate. Kubernetes provides built-in service discovery through DNS and services.

The choice depends on your infrastructure (Kubernetes vs traditional VMs), consistency requirements, operational complexity, and ecosystem integration. Kubernetes environments often use native service discovery, while traditional environments might use Eureka or Consul.

## Code Example

```java
// Consul service discovery
@SpringBootApplication
@EnableDiscoveryClient
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}

// application.yml for Consul
spring:
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        service-name: user-service
        health-check-path: /actuator/health

// Kubernetes service discovery
// Services automatically discoverable via DNS
// http://user-service.default.svc.cluster.local
```

## 🎯 Interviewer Insight

The interviewer wants to test your awareness of the service discovery landscape and your ability to choose the right tool for different environments.

## ⚠️ Common Mistake
Using Eureka in Kubernetes environments where native service discovery would be simpler and more efficient.

## ⚡ Follow-up Questions

1. How does Kubernetes service discovery work?
2. What are the advantages of Consul over Eureka?
3. When would you choose Zookeeper for service discovery?

## 📚 Related Topics

- [How Eureka Works](./02-how-eureka-works.md)
- [Discovery Patterns](./06-discovery-patterns.md)
- [14. Deployment with Docker and Kubernetes](../14-deployment-docker-kubernetes/README.md)

## Navigation

[← Previous Question](./11-load-balancer-integration.md) | [Back to Section](./README.md)
