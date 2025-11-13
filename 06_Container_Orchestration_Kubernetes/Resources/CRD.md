
## ⚙️ **What is a CRD in Kubernetes?**

A **Custom Resource Definition (CRD)** is a Kubernetes object that allows you to **create your own resource types** beyond the built-in resources like Pods, Services, or Deployments.

* CRDs let you **define your own API objects**.
* Once defined, you can create **Custom Resources (CRs)** using your new type.
* This allows Kubernetes to manage **custom workloads or configurations**.

> Think of CRD as **adding a new Kubernetes resource type**, and CR as an **instance of that type**.

---

## 🧩 **Why Do We Need CRDs?**

* Extend Kubernetes without modifying the core code.
* Build **custom controllers and operators**.
* Automate **complex application workflows**.
* Standardize **custom applications across clusters**.

---

## 🔧 **CRD vs CR**

| Term                               | Meaning                                                         |
| ---------------------------------- | --------------------------------------------------------------- |
| **CRD (CustomResourceDefinition)** | Defines a **new resource type** (like Deployment or Pod)        |
| **CR (Custom Resource)**           | An **instance** of the custom resource (like a Pod of type CRD) |

---

## 📄 **Example: Create a CRD**

### YAML File: `crd-demo.yaml`

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: foos.example.com
spec:
  group: example.com
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                size:
                  type: integer
  scope: Namespaced
  names:
    plural: foos
    singular: foo
    kind: Foo
    shortNames:
      - f
```

### Apply the CRD:

```bash
kubectl apply -f crd-demo.yaml
```

✅ This creates a **new resource type**: `Foo`.

---

## 📄 **Example: Create a Custom Resource (CR)**

### YAML File: `cr-demo.yaml`

```yaml
apiVersion: example.com/v1
kind: Foo
metadata:
  name: foo-sample
spec:
  size: 3
```

### Apply the Custom Resource:

```bash
kubectl apply -f cr-demo.yaml
kubectl get foos
```

Output:

```
NAME         AGE
foo-sample   1m
```

✅ Now you have **your own resource instance** in Kubernetes.

---

## 🔄 **How CRD Works**

1. Define **CRD** → tells Kubernetes about a new resource type.
2. Create **CR** → instance of the new resource type.
3. Optional: Create a **controller/operator** to act on the CR and manage workloads.

```
CRD (Foo)  → defines type
      │
      ▼
CR (foo-sample) → instance of Foo
```

---

## 🔧 **Common Commands**

| Command                         | Description                           |
| ------------------------------- | ------------------------------------- |
| `kubectl get crd`               | List all CRDs in the cluster          |
| `kubectl describe crd <name>`   | Show CRD details                      |
| `kubectl get <custom-resource>` | List all CRs of that type             |
| `kubectl delete crd <name>`     | Delete the CRD (also deletes all CRs) |

---

## 🧠 **Use Cases of CRDs**

* **Operators**: Automate applications (e.g., MySQL Operator, Prometheus Operator).
* **Custom workflows**: Define app-specific resources.
* **Multi-tenant apps**: Define per-tenant configurations as custom resources.
* **Infrastructure as code**: Model cloud resources as Kubernetes objects.

---

## 🔍 **Visual Overview**

```
Kubernetes API
 ├─ Built-in resources: Pod, Deployment, Service
 └─ CRDs (CustomResourceDefinition)
       └─ Custom Resources (CRs) → Foo, Bar, etc.
```

---

## ✅ **Summary**

| Concept      | Description                                           |
| ------------ | ----------------------------------------------------- |
| **CRD**      | CustomResourceDefinition, defines a new resource type |
| **CR**       | Custom Resource, instance of a CRD                    |
| **Use Case** | Extend Kubernetes API, Operators, custom workflows    |
| **Commands** | `kubectl get crd`, `kubectl get <custom-resource>`    |
| **Scope**    | Namespaced or Cluster-wide                            |

---
