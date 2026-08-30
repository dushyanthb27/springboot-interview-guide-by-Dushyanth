# How do you implement blue-green deployments? 🟠

## Question

How do you implement blue-green deployments in Kubernetes?

## Answer

Blue-green deployment maintains two identical production environments (blue and green). The new version is deployed to the green environment while the blue environment continues serving traffic. Once the green environment is verified, traffic is switched to it, and the blue environment can be updated or decommissioned.

Implementation involves creating two Deployments with different versions, using a Service to switch traffic between them, and using Ingress or Service updates to switch traffic after verification.

## Code Example

```yaml
# Blue deployment (current version)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: order-service-blue
spec:
  replicas: 3
  selector:
    matchLabels:
      app: order-service
      version: blue
  template:
    metadata:
      labels:
        app: order-service
        version: blue
    spec:
      containers:
      - name: order-service
        image: order-service:1.0

# Green deployment (new version)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: order-service-green
spec:
  replicas: 3
  selector:
    matchLabels:
      app: order-service
      version: green
  template:
    metadata:
      labels:
        app: order-service
        version: green
    spec:
      containers:
      - name: order-service
        image: order-service:2.0
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of blue-green deployment strategy and your ability to implement it in Kubernetes.

## ⚠️ Common Mistake
Not properly testing the green environment before switching traffic, leading to production issues.

## ⚡ Follow-up Questions

1. How do you switch traffic between blue and green?
2. What is the difference between blue-green and canary deployment?
3. How do you handle database migrations in blue-green deployments?

## 📚 Related Topics

- [Rolling Updates](./09-rolling-updates.md)
- [CI/CD Pipelines](./10-cicd-pipelines.md)
- [Deployment Best Practices](./13-deployment-best-practices.md)

## Navigation

[← Previous Question](./11-kubernetes-monitoring.md) | [Next Question →](./13-deployment-best-practices.md)
