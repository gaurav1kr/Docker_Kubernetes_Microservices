
# IBM Z s390x Provisioning and Deprovisioning Logic

This document explains the provisioning and deprovisioning lifecycle for **IBM Z (s390x) Bare Metal Servers** based on the architectural diagram provided.

---

## ⚙️ Provisioning Logic

### Step 1: Image Import
- An `Image` object is created.
- Triggers creation of:
  - `ZonalImage`
  - `StorageVolume`
  - `ExportPolicy`
  - `ExportPolicyRule`
- Managed by **Storage Controllers**.

### Step 2: BareMetalSpec Creation
- A `BareMetalSpec` object is created with a label pointing to the `ImageID`.

### Step 3: BareMetalServer Projection
- `BareMetalServer` resource is projected with the same `ImageID` label.

### Step 4: Read NFS Info
- `bm-server-controller` reads NFS info from the associated `StorageVolume`.

### Step 5: ExportPolicyRule Creation
- ExportPolicyRules are updated or created to allow NFS access from the provisioning pod.

### Step 6: Update BareMetalServer
- Server state updated by `bm-server-controller`.

### Step 7: Provision Action
- A `Provision` ServerAction is created by `bm-server-controller`.
- `bm-server-manager` processes the action.

### Step 8: REST Call to Fleetman
- `bm-server-manager` sends a POST request to `fleetman-rest-server` with NFS details.

### Step 9: Create Provisioning Pod
- `fleetman-rest-server` spawns a Kubernetes Job with a Provisioning Pod.
- The pod mounts the NFS volume.

### Step 10: Provision to Bare Metal Server
- Provisioning Pod:
  - Mounts NFS
  - Copies image to the local disk of the **Target Bare Metal Server**

---

## 🗑️ Deprovisioning Logic

### Step 11: Cleanup
- When `BareMetalServer` is deleted:
  - `ExportPolicyRule` is deleted.
  - NFS access is removed.
  - Additional cleanup logic (like image retention) may apply.

---

## 🧠 Key Components

| Component | Role |
|----------|------|
| **bm-server-controller** | Coordinates provisioning, updates state, reads NFS info |
| **bm-server-manager** | Processes provisioning actions and communicates with `fleetman` |
| **fleetman-rest-server** | Orchestrates provisioning pod creation |
| **Storage Controllers** | Manage NetApp export policies and volumes |
| **NetApp** | Exposes NFS volumes for image delivery |
| **Provisioning Pod** | Performs the image copy to Bare Metal machine |
| **Service node** | Runs provisioning pod, must have `genctl.role.service=true` |

---

## 📌 Notes
- Ensure correct `ExportPolicyRule` creation for successful NFS access.
- The Provisioning Pod is ephemeral and tied to image lifecycle.
