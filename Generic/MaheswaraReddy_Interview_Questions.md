
# 🧠 Interview Questions for Maheswara Reddy – Senior Technical Lead

**Profile Summary:**  
16+ years of experience with deep expertise in Kubernetes, Docker, Golang, CEPH SDS, Microservices, DevOps, and CI/CD. Certified CKA, CKAD, Docker Associate. Strong leadership and architectural design background.

---

## 🚀 Kubernetes & Platform Engineering

**Q1.** How does your team implement observability in a Kubernetes-based cloud storage platform?  
*Sample Answer:*  
"We integrated Prometheus and custom exporters for CEPH metrics, built Grafana dashboards, and used Loki for log collection. Alerts were managed using Alertmanager integrated with PagerDuty."

**Q2.** What are the trade-offs of running CEPH on Kubernetes?  
*Sample Answer:*  
"Running CEPH on K8s simplifies automation and lifecycle management but needs fine-tuned performance handling and affinity rules to avoid disk contention."

**Q3.** How do you manage persistent storage and failover in stateful microservices on OpenShift/Kubernetes?

---

## 🧰 Golang & Microservices

**Q4.** Can you explain how you implement and test gRPC services in Golang?  
*Sample Answer:*  
"We define proto contracts, generate stubs, use interceptors for observability, and run both unit and integration tests using mock servers."

**Q5.** Coding: Write a service in Go that receives a JSON config via POST and writes it to disk.

---

## 📦 Cloud Storage (CEPH, SDS)

**Q6.** What are the key failure scenarios you’ve handled in CEPH SDS?  
*Sample Answer:*  
"Failures include OSD down, monitor quorum issues, or pool misconfigurations. We built BOTs to detect and heal many of these automatically."

**Q7.** How would you design backup/restore for CEPH block storage in a multi-tenant OpenShift cluster?

---

## 🔐 CI/CD & DevSecOps

**Q8.** Describe your CI/CD pipeline for platform-level infrastructure.  
*Sample Answer:*  
"Jenkins pipelines with GitHub triggers, SonarQube, BlackDuck scanning, and Helm charts deployed via ArgoCD with GitOps principles."

**Q9.** What’s your approach to secret management for Kubernetes workloads?

---

## 🤝 Leadership & Collaboration

**Q10.** How do you mentor junior engineers across Dev + SRE teams?  
*Sample Answer:*  
"Stretch goals in sprints, pairing on complex PRs, and regular brown-bag sessions on storage/K8s internals."

**Q11.** Describe a cross-team conflict and how you resolved it.

---

## 🧪 Scenario-Based

**Q12.** CEPH backend slows under high I/O. Walk through your troubleshooting and remediation approach.

**Q13.** A logging POD hits OOM every hour after a recent feature push. What’s your debugging process?

---

## 💻 Optional Coding Task

> Implement a REST API in Go to expose CEPH disk health status. Bonus: export metrics for Prometheus scraping.

---

## ✅ Summary

**Core Areas to Cover:**
- Kubernetes internals (storage, observability, scaling)
- CEPH SDS automation & failures
- Golang microservices design & testing
- CI/CD (Jenkins, ArgoCD, BlackDuck, SonarQube)
- Leadership & cross-functional collaboration
