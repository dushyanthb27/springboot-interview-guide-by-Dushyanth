# What are the alternatives to Spring Cloud Gateway? 🟡

## Question

What are other API Gateway solutions besides Spring Cloud Gateway and when would you use them?

## Answer

Alternatives to Spring Cloud Gateway include Netflix Zuul (deprecated), Kong, Apigee, AWS API Gateway, NGINX, and Traefik. Each has different strengths: Kong offers rich plugin ecosystem, Apigee provides enterprise features, AWS API Gateway integrates with AWS services, NGINX provides high performance, and Traefik offers automatic service discovery.

The choice depends on requirements like cloud provider, feature set, operational complexity, and ecosystem integration. Kubernetes environments often use Traefik or NGINX Ingress Controller, while AWS environments might use AWS API Gateway.

## Code Example

```java
// Kong Gateway configuration (YAML)
# kong.yml
_format_version: "1.1"
services:
  - name: user-service
    url: http://user-service:8080
    routes:
      - name: user-service-route
        paths:
          - /api/users
plugins:
  - name: rate-limiting
    config:
      minute: 100
      policy: redis
```

## 🎯 Interviewer Insight

The interviewer wants to test your awareness of the API Gateway landscape and your ability to choose the right solution for different environments.

## ⚠️ Common Mistake
Using deprecated solutions like Netflix Zuul in new projects instead of Spring Cloud Gateway.

## ⚡ Follow-up Questions

1. What are the advantages of Kong over Spring Cloud Gateway?
2. When would you choose AWS API Gateway?
3. How does Traefik compare to Spring Cloud Gateway?

## 📚 Related Topics

- [How Spring Cloud Gateway Works](./02-how-spring-cloud-gateway-works.md)
- [Routing Patterns](./09-routing-patterns.md)
- [14. Deployment with Docker and Kubernetes](../14-deployment-docker-kubernetes/README.md)

## Navigation

[← Previous Question](./11-api-versioning.md) | [Back to Section](./README.md)
