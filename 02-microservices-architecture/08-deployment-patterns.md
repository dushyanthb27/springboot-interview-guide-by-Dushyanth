# What are the different types of microservices deployment patterns? 🟡

## Question

Explain the various deployment patterns for microservices and their advantages and disadvantages.

## Answer

Microservices deployment patterns include: Multiple Service Instances per Host (run multiple service instances on same physical/virtual host), Service Instance per Host (run one service instance per host), Service Instance per VM (package each service as a VM image), Service Instance per Container (package each service as a container, most popular), Serverless deployment (deploy services as functions), and Service Deployment Platform (use platforms like Kubernetes).

Container-based deployment (Docker) is most common due to resource efficiency, fast startup times, and ecosystem support. Kubernetes provides orchestration for containerized services with features like service discovery, load balancing, auto-scaling, and self-healing. Serverless deployment offers maximum scalability and minimal operational overhead but has limitations around state, execution time, and vendor lock-in.

## Code Example

```java
// Docker-based deployment pattern
// Dockerfile for a Spring Boot microservice
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY target/user-service.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]

// Kubernetes deployment configuration
apiVersion: apps/v1
kind: Deployment
metadata:
  name: user-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: user-service
  template:
    metadata:
      labels:
        app: user-service
    spec:
      containers:
      - name: user-service
        image: user-service:1.0.0
        ports:
        - containerPort: 8080
        env:
        - name: SPRING_PROFILES_ACTIVE
          value: "prod"
        resources:
          requests:
            memory: "512Mi"
            cpu: "500m"
          limits:
            memory: "1Gi"
            cpu: "1000m"
---
apiVersion: v1
kind: Service
metadata:
  name: user-service
spec:
  selector:
    app: user-service
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
  type: LoadBalancer

// Spring Boot configuration for containerization
@Configuration
public class ContainerConfig {

    @Bean
    public HealthIndicator containerHealthIndicator() {
        return () -> Health.up()
                .withDetail("container", System.getenv().getOrDefault("HOSTNAME", "unknown"))
                .build();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of deployment strategies and your familiarity with modern containerization and orchestration platforms. They're looking for awareness of trade-offs between different deployment approaches.

## ⚠️ Common Mistake

Not understanding the operational complexity of managing many microservices compared to a monolith. Another mistake is choosing deployment patterns without considering the organization's operational maturity and requirements.

## ⚡ Follow-up Questions

1. How do you handle configuration management in containerized deployments?
2. What are the advantages of using Kubernetes over Docker Compose?
3. How do you implement blue-green deployments for microservices?

## 📚 Related Topics

- [14. Deployment with Docker and Kubernetes](../14-deployment-docker-kubernetes/README.md)
- [Monitoring and Observability](./13-monitoring-observability.md)
- [06. Configuration Management](../06-configuration-management/README.md)

## Navigation

[← Previous Question](./07-service-boundaries.md) | [Next Question →](./09-cross-cutting-concerns.md)
