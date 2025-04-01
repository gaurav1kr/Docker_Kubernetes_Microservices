
# Custom Controller / Operator in Kubernetes – Explained with Golang Example

## 🧠 What is a Custom Controller / Operator?

### ✅ Controller
- Watches Kubernetes resources (e.g., Pods, Deployments)
- Ensures actual cluster state matches the desired state

### ✅ Operator
- A controller built around a **Custom Resource Definition (CRD)**
- Encodes domain-specific operational knowledge (e.g., deploy database, backups)

> **Operator = Custom Controller + CRD**

---

## ⚙️ Basic Architecture

1. **CRD (Custom Resource Definition)** – defines new resource type (e.g., `MyApp`)
2. **Controller** – watches `MyApp` resources and reconciles the actual state
3. **Reconciliation Loop** – ensures actual state matches the spec
4. **Client Libraries** – typically use `client-go` or `kubebuilder`

---

## 🧪 Minimal Example: Custom Controller in Go (Watches Pods)

This controller watches for Pod creation/deletion events and logs them.

### `main.go`

```go
package main

import (
    "context"
    "fmt"
    "log"
    "time"

    corev1 "k8s.io/api/core/v1"
    metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
    "k8s.io/client-go/kubernetes"
    "k8s.io/client-go/tools/cache"
    "k8s.io/client-go/tools/clientcmd"
    "k8s.io/client-go/informers"
)

func main() {
    config, err := clientcmd.BuildConfigFromFlags("", clientcmd.RecommendedHomeFile)
    if err != nil {
        panic(err)
    }

    clientset, err := kubernetes.NewForConfig(config)
    if err != nil {
        panic(err)
    }

    factory := informers.NewSharedInformerFactory(clientset, 30*time.Second)
    podInformer := factory.Core().V1().Pods().Informer()

    stopCh := make(chan struct{})
    defer close(stopCh)

    podInformer.AddEventHandler(cache.ResourceEventHandlerFuncs{
        AddFunc: func(obj interface{}) {
            pod := obj.(*corev1.Pod)
            fmt.Printf("New Pod Added: %s/%s\n", pod.Namespace, pod.Name)
        },
        DeleteFunc: func(obj interface{}) {
            pod := obj.(*corev1.Pod)
            fmt.Printf("Pod Deleted: %s/%s\n", pod.Namespace, pod.Name)
        },
    })

    factory.Start(stopCh)
    factory.WaitForCacheSync(stopCh)

    <-stopCh
}
```

---

## 🧰 Prerequisites

- Kubernetes cluster (Minikube or remote)
- Kubeconfig at `~/.kube/config`
- Golang 1.18+ installed
- Run `go mod init` and add dependencies for `client-go`

---

## ✅ Summary

This is a foundational example of a Kubernetes controller using Golang.
For production-grade Operators, consider using [`kubebuilder`](https://book.kubebuilder.io/).

