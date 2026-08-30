# How do you deploy Spring Boot to Kubernetes? 🟡

## Question

How do you deploy a Spring Boot application to Kubernetes?

## Answer

Spring Boot applications are deployed to Kubernetes using Deployment YAML files that define the desired state. The Deployment manages pod replicas, handles updates, and ensures the specified number of pods are running. Services provide network access to the pods.

Implementation involves creating a Deployment YAML with container image, replicas, and resource limits, creating a Service for network access, and applying the manifests with kubectl.

## Code Example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: order-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: order-service
  template:
    metadata:
      labels:
        app: order-service
    spec:
      containers:
      - name: order-service
        image: order-service:1.0
        ports:
        - containerPort: 8080
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Kubernetes Deployment configuration and your ability to deploy applications.

## ⚠️ Common Mistake
Not setting resource limits, leading to resource exhaustion in the cluster.

## ⚡ Follow-up Questions

1. How do you scale a Deployment?
2. What is the difference between replicas and pods?
3. How do you update a Deployment?

## 📚 Related Topics

- [Kubernetes Concepts](./05-kubernetes-concepts.md)
- [Kubernetes Services](./07-kubernetes-services.md)
- [Rolling Updates](./09-rolling-updates.md)

## Navigation

[← Previous Question](./05-kubernetes-concepts.md) | [Next Question →](./07-kubernetes-services.md)
