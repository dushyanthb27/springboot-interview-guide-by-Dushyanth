# 04. Service Discovery with Eureka

This section covers Netflix Eureka for service discovery in microservices architecture. Service discovery enables services to find and communicate with each other without hard-coded locations, essential for dynamic scaling and cloud-native applications.

## 📋 Prerequisites

- Understanding of microservices architecture (Section 02)
- Knowledge of Spring Boot fundamentals (Section 01)
- Familiarity with RESTful service communication
- Basic understanding of load balancing concepts

## 📚 Question Index

| # | Question | Difficulty |
|---|----------|------------|
| 01 | [What is service discovery and why is it important in microservices?](./01-what-is-service-discovery.md) | 🟢 |
| 02 | [How does Netflix Eureka work?](./02-how-eureka-works.md) | 🟡 |
| 03 | [How do you set up a Eureka server?](./03-eureka-server-setup.md) | 🟡 |
| 04 | [How do you register a service with Eureka?](./04-service-registration.md) | 🟡 |
| 05 | [How do clients discover services using Eureka?](./05-service-discovery-clients.md) | 🟡 |
| 06 | [What are the different service discovery patterns?](./06-discovery-patterns.md) | 🟡 |
| 07 | [How do you configure high availability for Eureka?](./07-high-availability.md) | 🟠 |
| 08 | [How do you implement health checks in Eureka?](./08-health-checks.md) | 🟡 |
| 09 | [How do you handle service instances going down?](./09-service-down-handling.md) | 🟠 |
| 10 | [What are the security considerations for Eureka?](./10-security.md) | 🟠 |
| 11 | [How do you integrate Eureka with Spring Cloud Load Balancer?](./11-load-balancer-integration.md) | 🟠 |
| 12 | [What are the alternatives to Eureka?](./12-alternatives.md) | 🟡 |

## 🎯 Key Takeaways

- **Service Registration**: Services register their availability with Eureka server
- **Service Discovery**: Clients query Eureka to find available service instances
- **High Availability**: Eureka server clustering for resilience
- **Health Monitoring**: Automatic health checks and instance removal
- **Load Balancing**: Integration with Spring Cloud Load Balancer
- **Self-Preservation**: Eureka's mechanism to handle network partitions

## 🔗 Related Sections

- [02. Microservices Architecture](../02-microservices-architecture/README.md) - Service discovery context
- [05. API Gateway Patterns](../05-api-gateway-patterns/README.md) - Gateway integration
- [08. Spring Rest Client](../08-spring-rest-client/README.md) - Client-side discovery

## 📖 Resources

- [Spring Cloud Netflix Eureka](https://cloud.spring.io/spring-cloud-netflix/reference/html/#spring-cloud-eureka-server)
- [Eureka Documentation](https://github.com/Netflix/eureka/wiki)
- [Spring Cloud Service Discovery](https://spring.io/projects/spring-cloud)

## 💡 Study Tips

1. **Understand the problem**: Why service discovery is needed in microservices
2. **Practice setup**: Create a Eureka server and register services
3. **Test discovery**: Implement client-side service discovery
4. **Configure HA**: Set up Eureka server clustering
5. **Monitor health**: Understand health check mechanisms

## 🎓 Difficulty Breakdown

- **🟢 Beginner (1 question)**: Basic concepts
- **🟡 Intermediate (8 questions)**: Configuration and implementation
- **🟠 Advanced (3 questions)**: High availability and security

Service discovery is fundamental to microservices architecture, enabling dynamic scaling and resilience. Master Eureka to build robust distributed systems.
