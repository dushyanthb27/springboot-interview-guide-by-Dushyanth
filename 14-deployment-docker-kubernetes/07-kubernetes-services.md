# How do you configure Kubernetes Services and Ingress? 🟡

## Question

How do you configure Kubernetes Services for internal communication and Ingress for external access?

## Answer

Kubernetes Services provide stable network endpoints for pods, enabling load balancing and service discovery. Service types include ClusterIP (internal), NodePort (exposed on node ports), and LoadBalancer (cloud load balancer). Ingress provides HTTP/HTTPS routing to services based on host and path rules.

Configuration involves creating Service YAMLs with selectors and ports, and Ingress YAMLs with routing rules. Services enable internal communication, while Ingress provides external access.

## Code Example

```yaml
apiVersion: v1
kind: Service
metadata:
  name: order-service
spec:
  selector:
    app: order-service
  ports:
  - port: 80
    targetPort: 8080
  type: ClusterIP

---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: api-ingress
spec:
  rules:
  - host: api.example.com
    http:
      paths:
      - path: /orders
        pathType: Prefix
        backend:
          service:
            name: order-service
            port:
              number: 80
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Kubernetes networking and your ability to configure Services and Ingress.

## ⚠️ Common Mistake
Not understanding the difference between Service types, leading to inappropriate exposure of services.

## ⚡ Follow-up Questions

1. What is the difference between ClusterIP and NodePort?
2. How does Ingress differ from LoadBalancer?
3. How do you configure TLS for Ingress?

## 📚 Related Topics

- [Kubernetes Deployment](./06-kubernetes-deployment.md)
- [Kubernetes Config](./08-kubernetes-config.md)
- [Kubernetes Secrets](./14-kubernetes-secrets.md)

## Navigation

[← Previous Question](./06-kubernetes-deployment.md) | [Next Question →](./08-kubernetes-config.md)
