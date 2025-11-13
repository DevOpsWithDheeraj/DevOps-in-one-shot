
## ⚙️ **What is RBAC in Kubernetes?**

**RBAC (Role-Based Access Control)** is a mechanism to **control who can do what in a Kubernetes cluster**.

* Grants **permissions based on roles**.
* Applies to **users, groups, or service accounts**.
* Uses **Roles, ClusterRoles, RoleBindings, and ClusterRoleBindings**.

> Think of RBAC as **security rules** for Kubernetes resources.

---

## 🧩 **Why Do We Need RBAC?**

* Limit access to **sensitive resources**.
* Enforce **least privilege principle**.
* Different teams may have **different permissions** (e.g., dev vs ops).
* Prevent accidental or malicious changes in the cluster.

---

## 🔧 **RBAC Components**

| Component              | Scope     | Description                                                       |
| ---------------------- | --------- | ----------------------------------------------------------------- |
| **Role**               | Namespace | Defines permissions within a namespace                            |
| **ClusterRole**        | Cluster   | Defines permissions cluster-wide                                  |
| **RoleBinding**        | Namespace | Grants a Role to a user/group/service account in a namespace      |
| **ClusterRoleBinding** | Cluster   | Grants a ClusterRole to a user/group/service account cluster-wide |

---

## 🧩 **Example 1: Role and RoleBinding**

### Role: `role-dev.yaml`

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: dev
  name: pod-reader
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list", "watch"]
```

### RoleBinding: `rolebinding-dev.yaml`

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods-binding
  namespace: dev
subjects:
  - kind: User
    name: dheeraj
    apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

✅ This allows user **`dheeraj`** to **read pods** in the `dev` namespace only.

---

## 🧩 **Example 2: ClusterRole and ClusterRoleBinding**

### ClusterRole: `clusterrole.yaml`

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-admin-read
rules:
  - apiGroups: [""]
    resources: ["pods", "nodes"]
    verbs: ["get", "list", "watch"]
```

### ClusterRoleBinding: `clusterrolebinding.yaml`

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: read-cluster-binding
subjects:
  - kind: User
    name: dheeraj
    apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: cluster-admin-read
  apiGroup: rbac.authorization.k8s.io
```

✅ This allows user **`dheeraj`** to **read pods and nodes across the entire cluster**.

---

## 🔄 **How RBAC Works**

```
User / Group / ServiceAccount
           │
           ▼
    Role / ClusterRole
           │
           ▼
   RoleBinding / ClusterRoleBinding
           │
           ▼
   Allowed Resources + Actions (verbs)
```

* **Verbs** = actions like `get`, `list`, `watch`, `create`, `update`, `delete`.
* **Resources** = Kubernetes objects like `pods`, `deployments`, `nodes`.

---

## 🔧 **Common Commands**

| Command                                   | Description                           |
| ----------------------------------------- | ------------------------------------- |
| `kubectl get roles -n <namespace>`        | List roles in namespace               |
| `kubectl get rolebindings -n <namespace>` | List role bindings in namespace       |
| `kubectl get clusterroles`                | List cluster roles                    |
| `kubectl get clusterrolebindings`         | List cluster role bindings            |
| `kubectl auth can-i <verb> <resource>`    | Check if a user can perform an action |

Example:

```bash
kubectl auth can-i get pods --as=dheeraj
```

---

## 🧠 **Best Practices**

* Follow **least privilege principle**: give only necessary permissions.
* Use **Namespaces + Roles** to separate team access.
* Use **ServiceAccounts** for applications instead of user accounts.
* Review **ClusterRoleBindings** carefully; they grant **cluster-wide permissions**.

---

## 🔍 **Visual Overview**

```
      User / Group / ServiceAccount
               │
         Role / ClusterRole
               │
      RoleBinding / ClusterRoleBinding
               │
          Resources + Verbs
```

* **Roles** = permissions in a namespace
* **ClusterRoles** = permissions cluster-wide
* **Bindings** = assign roles to users/groups/service accounts

---

✅ **Summary**

| Concept                              | Description                                           |
| ------------------------------------ | ----------------------------------------------------- |
| **RBAC**                             | Role-Based Access Control in Kubernetes               |
| **Role / ClusterRole**               | Define what actions are allowed                       |
| **RoleBinding / ClusterRoleBinding** | Assign roles to users/groups/service accounts         |
| **Verbs**                            | Actions like get, list, create, delete                |
| **Best Practice**                    | Least privilege, use namespaces, use service accounts |

---
