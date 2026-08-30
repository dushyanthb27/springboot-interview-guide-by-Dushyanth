# How do you handle secrets in Kubernetes? 🟡

## Question

How do you manage secrets in Kubernetes for sensitive data like passwords and API keys?

## Answer

Kubernetes Secrets store sensitive data like passwords, API keys, and certificates. Secrets are encoded in base64 and can be mounted as volumes or exposed as environment variables. For production, consider using external secret management tools like HashiCorp Vault or cloud provider secret managers.

Implementation involves creating Secret YAMLs, mounting them in pods, or using them as environment variables. Secrets should never be committed to version control.

## Code Example

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: YXBwX3VzZXI=
  password: cGFzc3dvcmQ=

---
apiVersion: v1
kind: Pod
metadata:
  name: my-app
spec:
  containers:
  - name: app
    image: my-app:1.0
    env:
    - name: DB_USERNAME
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: username
    - name: DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: password
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Kubernetes Secrets and your ability to manage sensitive data securely.

## ⚠️ Common Mistake
Committing Secrets to version control, creating security vulnerabilities.

## ⚡ Follow-up Questions

1. How do you create Secrets from files?
2. What is the difference between Opaque and TLS Secrets?
3. How do you integrate with external secret managers?

## 📚 Related Topics

- [Kubernetes Config](./08-kubernetes-config.md)
- [Kubernetes Deployment](./06-kubernetes-deployment.md)
- [09. Security with OAuth2](../09-security-oauth2/README.md)

## Navigation

[← Previous Question](./13-deployment-best-practices.md) | [Back to Section](./README.md)
