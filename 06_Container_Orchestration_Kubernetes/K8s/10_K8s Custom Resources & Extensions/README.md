
# 🚀 **Kubernetes Custom Resources & Extensions (CRD, Controller, Operator)**

Kubernetes is built with a philosophy:
**“Everything is a resource and managed by a controller.”**

But sometimes, the built-in resources (Pods, Deployments, Services…) are not enough.
For example, what if you want Kubernetes to:

* Manage a custom database life cycle?
* Create a custom type of application?
* Rotate TLS certificates automatically?
* Run custom business logic?

To achieve this, Kubernetes allows extensions using:

### ✔ **CRD (Custom Resource Definition)**

### ✔ **Controller**

### ✔ **Operator**

These three together allow you to turn Kubernetes into your own automation platform.

---

# 1️⃣ **Custom Resource (CR)**

A **Custom Resource** is a new type of resource you add to Kubernetes.

Example:
You create a resource called **MyApp** (your own kind of application) with fields like replicas, version, config, etc.

A CR is just **data in etcd**—Kubernetes stores it but does *nothing automatically* unless you write logic for it.

---

# 2️⃣ **CRD — Custom Resource Definition**

CRD = Schema/blueprint that defines your Custom Resource.

You install a CRD into the cluster → Kubernetes API Server automatically supports a new resource like:

```
kubectl get myapps
```

CRD defines:

* Name of the resource
* Fields & schema validation
* Version of the API

### 📝 Example CRD (myapp CRD)

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: myapps.example.com
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
                replicas:
                  type: integer
                image:
                  type: string
  scope: Namespaced
  names:
    plural: myapps
    singular: myapp
    kind: MyApp
    shortNames:
      - ma
```

After applying CRD:

```
kubectl get myapps
```

---

# 3️⃣ **Controller**

A **Controller** is a loop that continuously watches the Custom Resource and takes actions.

Think of it like:

> "When someone creates or modifies *MyApp*, what should Kubernetes do automatically?"

Controllers do:

* Watch for CR events
* Compare **desired state** (from CR) vs **actual state** (cluster)
* Reconcile differences

**Example**
If MyApp.spec.replicas = 3
→ Controller ensures 3 Pods are always running.

Controllers are generally written in Golang using **Kubebuilder** or **Operator SDK**.

---

# 4️⃣ **Operator**

Operator = CRD + Controller + Business Logic

It is basically a **domain-specific automation software** running inside Kubernetes.

Operators do smarter things than regular controllers.

### 🧠 Operators include:

* Auto-healing
* Backups
* Scaling
* Upgrades
* Status checks
* Auto-configuration
* Database lifecycle automation

They make Kubernetes behave like a human SRE.

### Example Operators:

* **Prometheus Operator**
* **Cert-Manager**
* **ElasticSearch Operator**
* **Kafka Operator**
* **MongoDB Operator**

---

# 🧩 How CRD, Controller & Operator Work Together

| Component                | Purpose                                |
| ------------------------ | -------------------------------------- |
| **CRD**                  | Adds new resource type to API Server   |
| **Custom Resource (CR)** | Actual instance of the custom type     |
| **Controller**           | Watches CR & takes actions             |
| **Operator**             | Controller + intelligence + automation |

---

# 📦 **Full Example — "MyApp Operator"**

## Step 1: Install CRD

You install `myapps.example.com` CRD (shown above).

## Step 2: Create a Custom Resource

```yaml
apiVersion: example.com/v1
kind: MyApp
metadata:
  name: dheeraj-app
spec:
  replicas: 2
  image: nginx:latest
```

Apply:

```
kubectl apply -f myapp.yaml
```

Now a new “desired state” exists in the cluster.

---

## Step 3: Controller Logic (simplified)

Your controller continuously checks:

```
Is there a MyApp CR?
If yes, read spec.replicas & spec.image
Create or update Deployment for MyApp
Ensure correct number of Pods always running
Restart Pods when image changes
```

Internally it may generate a Deployment like:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: dheeraj-app-deploy
spec:
  replicas: 2
  template:
    spec:
      containers:
        - name: app
          image: nginx:latest
```

---

## Step 4: Operator Intelligence

If this were a proper Operator, it may also:

* Auto-scale MyApp based on metrics
* Create ConfigMap/Secrets
* Roll back on failure
* Maintain version history
* Send events/status updates

This is why we say:

> **Operator = CRD + Controller + Automation logic**

---

# 🧠 **Simple Analogy**

| Concept        | Analogy                                                                                    |
| -------------- | ------------------------------------------------------------------------------------------ |
| **CRD**        | Registering a new Form Type at an Office                                                   |
| **CR**         | One filled-out form submitted by a person                                                  |
| **Controller** | Officer who checks the form and performs actions                                           |
| **Operator**   | Super-smart officer who also fixes issues, maintains records, does backups, upgrades, etc. |

---

# 🎯 Final Summary (Interview Ready)

### **CRD**

* Defines new API resource types.
* Adds extensibility to Kubernetes.

### **Custom Resource**

* Actual instance of the CRD created by users.

### **Controller**

* Reconciliation loop that reads CR and applies desired changes.

### **Operator**

* Application-specific controller that automates complex operational tasks.
* Implements human SRE knowledge inside Kubernetes.

---
