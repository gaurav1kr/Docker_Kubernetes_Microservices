
# Service Discovery and Communication Between Microservices

This guide outlines best practices for handling service discovery and inter-service communication in Kubernetes-based microservice architectures.

---

## 🔎 1. Service Discovery

### ✅ Kubernetes-native Discovery
- Built-in via DNS (`kube-dns` or `CoreDNS`)
- Each service gets a DNS entry

### 🛠️ Example:
```bash
curl http://user-service.default.svc.cluster.local:8080
```

---

## 🔌 2. Communication Protocols

### ✅ REST (HTTP/JSON)
- Simple and human-readable
- Ideal for external-facing APIs

### ✅ gRPC (HTTP/2 + Protobuf)
- High performance and strongly typed
- Best for internal services with strict contracts

### 🛠️ gRPC Example:
```proto
service UserService {
  rpc GetUser(UserRequest) returns (UserResponse);
}
```

> Use `protoc` to generate code stubs for different languages.

---

## 🕸️ 3. Service Mesh (e.g., Istio, Linkerd)

### Benefits:
- Transparent service discovery and load balancing
- Advanced traffic routing, retries, observability, and mTLS
- Offloads comms logic from application code

### 🛠️ Istio VirtualService Example:
```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: user-service
spec:
  hosts:
  - user-service
  http:
  - route:
    - destination:
        host: user-service
        subset: v2
```

---

## 🔒 4. Secure Communication (mTLS)

- Enforced via service mesh sidecars (e.g., Envoy in Istio)
- Eliminates the need for app-level TLS config

---

## 🔁 5. Load Balancing & Resilience

| Feature          | Kubernetes | Service Mesh |
|------------------|------------|---------------|
| Load Balancing   | kube-proxy (L4) | Envoy (L7) |
| Retries/Timeouts | App-level  | Mesh-level (declarative) |
| Circuit Breaking | Manual     | Built-in     |
| Rate Limiting    | External tools | Native via CRDs |

---

## ✅ Summary Comparison

| Aspect                     | Kubernetes-native | gRPC/REST | Service Mesh |
|---------------------------|-------------------|-----------|---------------|
| Service Discovery         | DNS-based         | DNS/DNS+LB| Auto-injected |
| Protocol Support          | HTTP              | REST/gRPC | REST/gRPC     |
| Load Balancing            | kube-proxy        | App/Client| Envoy (L7)    |
| Security (mTLS)           | Manual setup      | Manual    | Built-in      |
| Observability             | Custom            | Custom    | Prometheus + Jaeger |
| Traffic Routing / Canary  | Manual             | App logic | Declarative via CRDs |

