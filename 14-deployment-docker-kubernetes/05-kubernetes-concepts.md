# What are the key Kubernetes concepts? 🟡

## Question

What are the key Kubernetes concepts and how do they relate to each other?

## Answer

Key Kubernetes concepts include Pods (smallest deployable units), Services (network abstraction for pods), Deployments (manages pod replicas), ConfigMaps (configuration data), Secrets (sensitive data), Namespaces (resource isolation), and Ingress (HTTP routing). These concepts work together to provide container orchestration.

Pods contain one or more containers. Services provide stable network endpoints. Deployments manage pod lifecycle and scaling. ConfigMaps and Secrets provide configuration. Namespaces provide resource isolation. Ingress provides external access.

## Code Example

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app
spec:
  containers:
  - name: app
    image: my-app:1.0
    ports:
    - containerPort: 8080
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Kubernetes core concepts and their relationships.

## ⚠️ Common Mistake
Confusing Pods with Deployments, not understanding the abstraction layers.

## ⚡ Follow-up Questions

1. What is the difference between a Pod and a Deployment?
2. How do Services provide load balancing?
3. What is the purpose of Namespaces?

## 📚 Related Topics

- [Kubernetes Deployment](./06-kubernetes-deployment.md)
- [Kubernetes Services](./07-kubernetes-services.md)
- [Kubernetes Config](./08-kubernetes-config.md)

## Navigation

[← Previous Question](./04-docker-compose.md) | [Next Question →](./06-kubernetes-deployment.md)
