# How do you create a Dockerfile for Spring Boot? 🟡

## Question

How do you create an optimized Dockerfile for a Spring Boot application?

## Answer

A Dockerfile for Spring Boot uses multi-stage builds to optimize image size. The build stage compiles the application, and the runtime stage uses a lightweight JRE image. This reduces image size by excluding build tools and dependencies not needed at runtime.

Implementation involves using a multi-stage Dockerfile, copying the JAR file, setting the entry point, and exposing the application port.

## Code Example

```dockerfile
# Build stage
FROM maven:3.8-openjdk-17 AS build
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests

# Runtime stage
FROM openjdk:17-slim
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Dockerfile creation and optimization for Spring Boot applications.

## ⚠️ Common Mistake
Not using multi-stage builds, resulting in unnecessarily large image sizes.

## ⚡ Follow-up Questions

1. What is a multi-stage build?
2. How do you reduce Docker image size?
3. What is the difference between ENTRYPOINT and CMD?

## 📚 Related Topics

- [Docker Optimization](./03-docker-optimization.md)
- [Docker Compose](./04-docker-compose.md)
- [Kubernetes Deployment](./06-kubernetes-deployment.md)

## Navigation

[← Previous Question](./01-containerization.md) | [Next Question →](./03-docker-optimization.md)
