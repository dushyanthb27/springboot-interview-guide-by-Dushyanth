# How do you use Docker Compose for local development? 🟡

## Question

How do you use Docker Compose to set up a local development environment?

## Answer

Docker Compose defines and runs multi-container Docker applications. It's ideal for local development environments with multiple services (application, database, message broker). The docker-compose.yml file defines services, networks, and volumes for the development environment.

Configuration involves defining services in docker-compose.yml, configuring environment variables and volumes, and using docker-compose up to start the environment.

## Code Example

```yaml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      - SPRING_DATASOURCE_URL=jdbc:postgresql://db:5432/app
    depends_on:
      - db
      - redis

  db:
    image: postgres:14
    environment:
      - POSTGRES_DB=app
      - POSTGRES_USER=app
      - POSTGRES_PASSWORD=app
    volumes:
      - postgres-data:/var/lib/postgresql/data

  redis:
    image: redis:7

volumes:
  postgres-data:
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Docker Compose and your ability to set up development environments.

## ⚠️ Common Mistake
Not configuring proper dependencies between services, leading to startup failures.

## ⚡ Follow-up Questions

1. How do you scale services with Docker Compose?
2. What is the difference between Docker and Docker Compose?
3. How do you manage environment variables?

## 📚 Related Topics

- [Dockerfile](./02-dockerfile.md)
- [Docker Optimization](./03-docker-optimization.md)
- [Kubernetes Deployment](./06-kubernetes-deployment.md)

## Navigation

[← Previous Question](./03-docker-optimization.md) | [Next Question →](./05-kubernetes-concepts.md)
