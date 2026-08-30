# How does Netflix Eureka work? 🟡

## Question

Explain how Netflix Eureka implements service discovery and the key components involved.

## Answer

Netflix Eureka is a REST-based service for locating services for the purpose of load balancing and failover of middle-tier servers. It consists of Eureka Server (service registry that maintains information about available service instances) and Eureka Clients (services that register with Eureka and query it to discover other services).

Eureka works through a periodic heartbeat mechanism where clients send heartbeats to the server to indicate they're alive. If the server doesn't receive heartbeats for a certain period, it removes the instance from the registry. Eureka also supports caching on the client side to improve performance and resilience.

## Code Example

```java
// Eureka Server
@SpringBootApplication
@EnableEurekaServer
public class EurekaServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }
}

// Eureka Client
@SpringBootApplication
@EnableDiscoveryClient
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}

// Service discovery usage
@Service
public class OrderService {
    private final DiscoveryClient discoveryClient;

    public OrderService(DiscoveryClient discoveryClient) {
        this.discoveryClient = discoveryClient;
    }

    public List<String> getUserServiceInstances() {
        List<ServiceInstance> instances = discoveryClient.getInstances("user-service");
        return instances.stream()
                .map(ServiceInstance::getUri)
                .map(Object::toString)
                .collect(Collectors.toList());
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Eureka's architecture and the heartbeat mechanism that maintains service availability information.

## ⚠️ Common Mistake
Not understanding that Eureka clients cache service information locally, which can lead to stale data during network partitions.

## ⚡ Follow-up Questions

1. What is the self-preservation mode in Eureka?
2. How does Eureka handle network partitions?
3. What is the difference between Eureka and Consul?

## 📚 Related Topics

- [Eureka Server Setup](./03-eureka-server-setup.md)
- [Service Registration](./04-service-registration.md)
- [High Availability](./07-high-availability.md)

## Navigation

[← Previous Question](./01-what-is-service-discovery.md) | [Next Question →](./03-eureka-server-setup.md)
