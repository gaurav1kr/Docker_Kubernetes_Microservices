
# Resilience Patterns in Microservices

This document outlines key patterns and best practices to implement resilience in microservices, ensuring they can handle failures gracefully.

---

## 🛡️ 1. Circuit Breaker

### 🔍 What It Does:
- Prevents repeated attempts to call a failing service.
- Opens circuit on error threshold, trips, then retries after cooldown.

### ✅ Tools:
- **Resilience4j**, **Envoy**, **Istio**, **sony/gobreaker** (Go)

### 🛠️ Example in Go:
```go
cb := gobreaker.NewCircuitBreaker(gobreaker.Settings{})
result, err := cb.Execute(func() (interface{}, error) {
    return http.Get("http://downstream-service")
})
```

---

## 🔁 2. Retry with Backoff

### 🔍 What It Does:
- Retries failed requests after delay.
- Uses exponential backoff + jitter to avoid spikes.

### ✅ Tools:
- **Resilience4j**, **Istio**, `avast/retry-go` (Go)

### 🛠️ Example in Go:
```go
retry.Do(func() error {
    resp, err := http.Get("http://downstream-service")
    return err
}, retry.Attempts(3), retry.Delay(time.Second))
```

---

## ⏱️ 3. Timeout

### 🔍 What It Does:
- Limits wait time for downstream service to respond.

### ✅ Tools:
- `http.Client` timeout, Istio, Envoy

### 🛠️ Example in Go:
```go
client := http.Client{Timeout: 2 * time.Second}
_, err := client.Get("http://slow-service")
```

---

## 🗳️ 4. Bulkhead

### 🔍 What It Does:
- Limits concurrent access to protect system resources.
- Isolates failures in different parts of the system.

### ✅ Tools:
- Resilience4j, Istio DestinationRules, Go channels

---

## 📈 5. Rate Limiting / Throttling

### 🔍 What It Does:
- Controls the number of allowed requests per second/minute.

### ✅ Tools:
- Istio RateLimit, Envoy, `golang.org/x/time/rate`

### 🛠️ Example in Go:
```go
limiter := rate.NewLimiter(1, 5) // 1 request/sec, burst of 5
if limiter.Allow() {
    // proceed with request
}
```

---

## 🧪 6. Fallback / Graceful Degradation

### 🔍 What It Does:
- Provides cached or default response when the real service fails.

### ✅ Tools:
- Resilience4j fallback, manual logic

---

## 🔁 Resilience with Istio

### 🛠️ Example Istio Config:
```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
spec:
  http:
  - route:
    - destination:
        host: myservice
    retries:
      attempts: 3
      perTryTimeout: 2s
      retryOn: gateway-error,connect-failure
```

---

## ✅ Summary Table

| Pattern           | Purpose                              | Tools                         |
|------------------|--------------------------------------|-------------------------------|
| Circuit Breaker   | Avoid overloading failing services   | Resilience4j, gobreaker, Istio |
| Retry             | Auto-retry failed requests           | retry-go, Istio               |
| Timeout           | Bound slow requests                  | http.Client, Istio            |
| Bulkhead          | Isolate failures, resource limits    | Channels, Resilience4j        |
| Rate Limiting     | Prevent overloads                    | Istio, Envoy, rate-limiter    |
| Fallback          | Gracefully handle failures           | Manual / cached fallback      |
