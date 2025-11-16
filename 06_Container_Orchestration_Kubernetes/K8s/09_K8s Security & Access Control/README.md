

# 🔐 **Kubernetes Security & Access Control (RBAC)**

Kubernetes follows a strong security model based on:

* **Authentication** → Who are you?
* **Authorization** → What are you allowed to do?
* **Admission Control** → Should this request be allowed?

The most important authorization mechanism in Kubernetes is **RBAC**.

---

# 🛡️ **RBAC — Role-Based Access Control**

RBAC controls **who can perform what action on which resource**.

In Kubernetes, RBAC determines permissions for:

* Users
* Groups
* Service Accounts

---

# 🧩 **RBAC Components**

RBAC is built using **4 primary objects**:

| RBAC Component         | Purpose                                                     |
| ---------------------- | ----------------------------------------------------------- |
| **Role**               | Permissions within a **single namespace**                   |
| **ClusterRole**        | Permissions across the **entire cluster**                   |
| **RoleBinding**        | Assigns a Role to a user/service account in **a namespace** |
| **ClusterRoleBinding** | Assigns a ClusterRole cluster-wide                          |

---

# 💡 **1. Role (Namespace Scope)**

A **Role** defines what actions a user can perform **inside a specific namespace**.

### Example Role: Allow reading Pods in namespace `dev`

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: dev
  name: pod-reader
rules:
- apiGroups: [""]         # "" means core API group (pods, services)
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```

---

# 💡 **2. RoleBinding (Bind Role → User)**

Assigns the above role to a specific user or service account.

### Example RoleBinding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods-binding
  namespace: dev
subjects:
- kind: User
  name: dheeraj   # the username
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

👉 Now **user dheeraj** can read pods only in the `dev` namespace.

---

# 🌍 **3. ClusterRole (Cluster-Wide Permissions)**

Used when permissions need to:

* Span across all namespaces
* Apply to **non-namespaced** resources
  (e.g., nodes, persistentvolumes)

### Example ClusterRole: Read Nodes and PersistentVolumes

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: node-reader
rules:
- apiGroups: [""]
  resources: ["nodes", "persistentvolumes"]
  verbs: ["get", "list", "watch"]
```

---

# 🌍 **4. ClusterRoleBinding (Bind → ClusterRole)**

Bind the above cluster role to a user.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: node-read-binding
subjects:
- kind: User
  name: dheeraj
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: node-reader
  apiGroup: rbac.authorization.k8s.io
```

👉 Now **dheeraj** can read nodes & PVs across **all namespaces**.

---

# 🎯 **RBAC Example Scenario (Complete Flow)**

### **Scenario:**

Team DevOps wants:

* **Read-only access** to all pods in namespace `dev`
* **Full access** to deployments in namespace `dev`
* **No access** to other namespaces

### Solution:

1️⃣ Create two roles:

* pod-reader
* deploy-admin

2️⃣ Bind them to user dheeraj using role bindings.

---

### **pod-reader Role**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: dev
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```

### **deploy-admin Role**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: dev
  name: deploy-admin
rules:
- apiGroups: ["apps"]
  resources: ["deployments"]
  verbs: ["get", "watch", "list", "create", "update", "delete"]
```

### **RoleBinding**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: devops-access
  namespace: dev
subjects:
- kind: User
  name: dheeraj
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: devops-deploy-access
  namespace: dev
subjects:
- kind: User
  name: dheeraj
roleRef:
  kind: Role
  name: deploy-admin
  apiGroup: rbac.authorization.k8s.io
```

✔️ Now **dheeraj** can:

* read pods in `dev`
* fully manage deployments in `dev`
* access NOTHING outside `dev`

---

# 🔒 Bonus: Service Accounts in RBAC

Service accounts are used by **applications running inside Pods**.

### Example: Create ServiceAccount + Bind Role

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: backend-sa
  namespace: dev
```

Bind permissions:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: backend-sa-binding
  namespace: dev
subjects:
- kind: ServiceAccount
  name: backend-sa
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

👉 The application running inside the pod using `backend-sa` can now read Pods.

---

# 🔐 RBAC Best Practices (Interview Tips)

* Give **least privilege** (never over-assign permissions).
* Use **namespaces** for multi-tenancy.
* For applications, use **ServiceAccounts**, never admin.
* Do NOT use **ClusterRoleBinding** if RoleBinding is enough.
* Avoid using the default `serviceaccount`.

---

# ✅ Summary Table

| RBAC Object            | Scope     | Used For                           |
| ---------------------- | --------- | ---------------------------------- |
| **Role**               | Namespace | Limited access                     |
| **RoleBinding**        | Namespace | Bind Role → user/SA                |
| **ClusterRole**        | Cluster   | Global or non-namespaced resources |
| **ClusterRoleBinding** | Cluster   | Bind cluster-wide role             |

---
