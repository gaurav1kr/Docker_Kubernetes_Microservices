
# Managing Secrets and Configuration for Microservices in Kubernetes

This guide explains how to securely and effectively manage configuration and secrets for microservices deployed on Kubernetes.

---

## 🔧 1. Kubernetes Native Config Management

### ✅ ConfigMaps
- For non-sensitive configuration like feature flags, URLs, ports.

### 🛠️ Example:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  LOG_LEVEL: "debug"
  SERVICE_URL: "http://user-service"
```

### In Deployment:
```yaml
envFrom:
- configMapRef:
    name: app-config
```

---

### 🔐 Secrets
- For sensitive data such as passwords, tokens, API keys.
- Base64-encoded (not encrypted at rest by default).

### 🛠️ Example:
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: YWRtaW4=
  password: cGFzc3dvcmQ=
```

---

## 🔁 2. Mounting Configurations in Pods

- As Environment Variables:
```yaml
env:
- name: DB_USER
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: username
```

- As Volumes:
```yaml
volumeMounts:
- mountPath: /etc/config
  name: config-volume

volumes:
- name: config-volume
  configMap:
    name: app-config
```

---

## 🧰 3. Advanced Tools for Secret Management

### 🔐 External Secret Stores

| Tool | Description |
|------|-------------|
| **HashiCorp Vault** | Dynamic secrets with policies and audit logs |
| **AWS Secrets Manager** | Managed secret store with rotation |
| **Azure Key Vault** | Secure storage for secrets/keys/certs |
| **GCP Secret Manager** | Google Cloud’s native secret store |

> Use **External Secrets Operator (ESO)** or **Vault Agent Injector** to sync secrets securely.

---

### 🛠️ Example with External Secrets Operator:
```yaml
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: db-secret
spec:
  secretStoreRef:
    name: vault-backend
    kind: ClusterSecretStore
  target:
    name: db-secret
  data:
  - secretKey: username
    remoteRef:
      key: prod/db-creds
      property: username
```

---

## 🛡️ 4. Best Practices

- 🔐 Never hardcode secrets in source code or YAML
- 🔒 Enable **encryption at rest** for Secrets
- 🛑 Use **RBAC** to control secret access
- 🔄 Rotate secrets periodically
- 🧼 Mount secrets as **read-only volumes**
- 👀 Audit access to secrets regularly

---

## ✅ Summary Table

| Purpose        | Tool/Method             | Usage                      |
|----------------|--------------------------|-----------------------------|
| Non-sensitive config | ConfigMap              | env vars / mounted files     |
| Secrets (static)     | Kubernetes Secrets      | env vars / volumes           |
| Secrets (dynamic)    | Vault, AWS Secrets, ESO | synced into K8s Secrets      |
| Security Enhancements| KMS, RBAC, Encryption   | secure storage and access    |
