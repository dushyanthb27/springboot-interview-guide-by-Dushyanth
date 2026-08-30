# How do you implement rolling updates in Kubernetes? 🟠

## Question

How do you implement rolling updates to deploy new versions without downtime?

## Answer

Rolling updates in Kubernetes gradually replace old pods with new pods, ensuring zero downtime deployments. Kubernetes creates new pods, waits for them to be healthy, then terminates old pods. This process continues until all pods are updated. Rolling updates can be configured with update strategies, readiness probes, and max unavailable settings.

Configuration involves setting the update strategy in Deployment YAML, configuring readiness and liveness probes, and setting maxSurge and maxUnavailable parameters to control the update process.

## Code Example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: order-service
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  template:
    spec:
      containers:
      - name: order-service
        image: order-service:2.0
        readinessProbe:
          httpGet:
            path: /actuator/health
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 10
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of rolling updates and your ability to implement zero-downtime deployments.

## ⚠️ Common Mistake
Not configuring readiness probes, leading to traffic being sent to pods that aren't ready.

## ⚡ Follow-up Questions

1. What is the difference between readiness and liveness probes?
2. How do you rollback a rolling update?
3. What is maxSurge and maxUnavailable?

## 📚 Related Topics

- [Kubernetes Deployment](./06-kubernetes-deployment.md)
- [Blue-Green Deployment](./12-blue-green-deployment.md)
- [CI/CD Pipelines](./10-cicd-pipelines.md)

## Navigation

[← Previous Question](./08-kubernetes-config.md) | [Next Question →](./10-cicd-pipelines.md)
