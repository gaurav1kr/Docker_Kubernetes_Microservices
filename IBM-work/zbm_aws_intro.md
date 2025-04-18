
## Cloud-Native Bare Metal Provisioning on IBM Z: Architecting for Scale and Security

Hi, I’m [Your Name], and today I’ll walk you through my architectural work in bringing **IBM Z Bare Metal (Z BM)** servers into a VPC-native cloud ecosystem. The focus was on enabling **cloud-native provisioning, management, and observability** for IBM LinuxONE systems — using secure APIs, Kubernetes controllers, and infrastructure automation.

---

### 🧩 Challenge

Z Systems, known for their performance and security (EAL5 certified), weren’t natively integrated into IBM Cloud’s VPC stack. My task was to design the control plane and provisioning pipeline for Z BMs — aligning it with VPC’s x86 Bare Metal experience but tailored for Z architecture (`s390x`).

---

### 🛠️ My Key Contributions

#### ⚙️ 1. z-lpar-manager (Controller for Z BMs)
- A Kubernetes-native controller responsible for Z BM lifecycle.
- Analogous to AWS’s internal controllers (like EC2 instance manager or Fargate orchestrators).
- Handles **create/start/stop/delete/reboot** using secure API calls to Z management entities (HMC, SAN).
- Secured via **mTLS**; credentials fetched from **Vault via sidecar injection**.

#### 🔌 2. z-mgmt-agent (Secure API Layer)
- Exposes a **RESTful API** layer to interact with underlying Z hardware.
- Think of it as an equivalent to AWS Systems Manager Agent for Z hardware.
- Handles:
  - Dynamic creation of virtual NICs using Hipersockets (like ENIs)
  - Secure disk provisioning, format, and LUKS encryption
  - Fetching ASCII console access (like AWS EC2 serial console)

#### 📦 3. Provisioner Flow (Image Deployment via VCN)
- A lightweight **ephemeral pod** called `z-provisioner` handles OS image delivery.
- Bootstraps a minimal ISO, then securely **copies the selected Linux image over an IPSec-enabled VCN interface** to the BM's FCP storage.
- Similar to how AWS may use SSM Run Command + user-data to bootstrap bare metal.

---

### 🌐 Cloud-Native Constructs
- Defined custom **Kubernetes CRDs** for Z-specific networking and server resources.
- Extended existing **BM APIs and Terraform providers** to support Z-specific attributes.
- Integrated with existing **CI/CD pipelines** to automate lifecycle validation and regression.

---

### ✅ Outcomes
- Delivered Z Bare Metal as a **first-class VPC-native workload**, just like EC2 Bare Metal.
- Customers can now consume mainframe-grade compute with the **same UX as standard x86 instances** — via API, CLI, and Terraform.
- Secured networking using **VF + Hipersocket** mappings, storage via **FCP SAN**, and image provisioning through **ephemeral control paths**.

---

This introduction positions my work in a cloud-native and AWS-relevant context, showcasing a strong grasp of hybrid architecture, secure provisioning, and control plane engineering at scale.
