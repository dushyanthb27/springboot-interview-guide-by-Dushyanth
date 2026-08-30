# How do you manage configuration in Kubernetes? 🟡

## Question

How do you manage application configuration in Kubernetes using ConfigMaps and Secrets?

## Answer

Kubernetes ConfigMaps store configuration data as key-value pairs, while Secrets store sensitive data like passwords and API keys. Both can be mounted as volumes or exposed as environment variables in pods. This separates configuration from container images, enabling better security and flexibility.

Configuration involves creating ConfigMap and Secret YAMLs, mounting them in pods, or using them as environment variables. Spring Boot can automatically load configuration from mounted ConfigMaps.

## Code Example

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  application.properties: |
    spring.datasource.url=jdbc:postgresql://db:5432/app
    server.port=8080

---
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  password: cGFzc3dvcmQ=
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Kubernetes configuration management and your ability to use ConfigMaps and Secrets.

## ⚠️ Common Mistake
Storing sensitive data in ConfigMaps instead of Secrets, creating security vulnerabilities.

## ⚡ Follow-up Questions

1. How do you mount ConfigMaps as volumes?
2. What is the difference between ConfigMaps and Secrets?
3. How does Spring Boot load Kubernetes ConfigMaps?

## 📚 Related Topics

- [Kubernetes Deployment](./06-kubernetes-deployment.md)
- [Kubernetes Secrets](./14-kubernetes-secrets.md)
- [06. Configuration Management](../06-configuration-management/README.md)

## Navigation

[← Previous Question](./07-kubernetes-services.md) | [Next Question →](./09-rolling-updates.md)
