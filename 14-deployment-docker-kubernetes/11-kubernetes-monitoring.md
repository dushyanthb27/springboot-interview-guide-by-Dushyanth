# How do you monitor Kubernetes deployments? 🟡

## Question

How do you monitor Kubernetes deployments and application health?

## Answer

Kubernetes monitoring involves monitoring cluster health, pod status, resource utilization, and application metrics. Tools like Prometheus and Grafana are commonly used for metrics collection and visualization. Kubernetes provides built-in metrics through Metrics Server and custom metrics.

Configuration involves setting up Prometheus to scrape Kubernetes metrics, configuring Grafana dashboards, and implementing health checks in applications using readiness and liveness probes.

## Code Example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: order-service
spec:
  template:
    spec:
      containers:
      - name: order-service
        image: order-service:1.0
        livenessProbe:
          httpGet:
            path: /actuator/health/liveness
            port: 8080
          initialDelaySeconds: 60
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /actuator/health/readiness
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 5
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Kubernetes monitoring and your ability to implement health checks.

## ⚠️ Common Mistake
Not implementing proper health checks, leading to unhealthy pods receiving traffic.

## ⚡ Follow-up Questions

1. What is the difference between liveness and readiness probes?
2. How does Prometheus collect Kubernetes metrics?
3. What metrics should you monitor?

## 📚 Related Topics

- [Kubernetes Deployment](./06-kubernetes-deployment.md)
- [Rolling Updates](./09-rolling-updates.md)
- [10. Distributed Tracing](../10-distributed-tracing/README.md)

## Navigation

[← Previous Question](./10-cicd-pipelines.md) | [Next Question →](./12-blue-green-deployment.md)
