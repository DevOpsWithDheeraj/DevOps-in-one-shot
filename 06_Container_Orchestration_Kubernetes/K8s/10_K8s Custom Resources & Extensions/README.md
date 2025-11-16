
# 🚀 **Kubernetes Custom Resources & Extensions (CRD, Controller, Operator)**

Kubernetes is designed to be **extensible**, meaning you can add your **own API components** just like built-in objects (Pods, Deployments, Services).

This extensibility comes mainly through:

✔ **Custom Resources (CR)**
✔ **Custom Resource Definitions (CRD)**
✔ **Controllers**
✔ **Operators**

Let’s understand each deeply.

---

# 1️⃣ **Custom Resources (CR) – What & Why?**

### **What is a Custom Resource?**

A **Custom Resource (CR)** is a new type of resource you add to your Kubernetes cluster.
It behaves like built-in ones such as Pods or Services.

Example:
You can create your own type:

* **Database**
* **Backup**
* **AppConfig**
* **TeamQuota**

### Why you need it?

To make Kubernetes understand **your own domain concepts**.

---

# 2️⃣ **Custom Resource Definition (CRD)**

### **What is a CRD?**

CRD = A schema that defines *your own API type* inside Kubernetes.

After creating a CRD, users can create objects of that type.

Example:
You create a CRD **Database** → Kubernetes API will now support:

```
apiVersion: myapps.example.com/v1
kind: Database
```

### Example CRD (YAML)

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: databases.myapps.example.com
spec:
  group: myapps.example.com
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
                dbName:
                  type: string
                size:
                  type: string
  scope: Namespaced
  names:
    plural: databases
    singular: database
    kind: Database
    shortNames:
      - db
```

After applying this CRD:

```
kubectl apply -f database-crd.yaml
```

Now you can create custom objects:

### Example Custom Resource (CR)

```yaml
apiVersion: myapps.example.com/v1
kind: Database
metadata:
  name: mydb
spec:
  dbName: usersDB
  size: small
```

So far it’s **just data**.
Something has to act on this data → this is where **Controllers** come.

---

# 3️⃣ **Controller – The Brain that Watches & Acts**

### **What is a Controller?**

A controller continuously watches:

* Your **Custom Resource**
* The actual state in the cluster

And tries to match **current state = desired state**.

Kubernetes uses controllers internally:

* Deployment Controller
* ReplicaSet Controller
* Node Controller, etc.

You can write your own controller for a CRD.

### Controller responsibilities:

✔ Watch for changes
✔ Take actions (create pods, configmaps, deploy DB)
✔ Ensure desired state

Example for our Database object:

* If `size: small`, controller creates a small DB Pod
* If CR changes to `size: medium`, controller updates the DB Pod
* If CR is deleted, it deletes the DB Pod

A CRD without a controller is like a **form without someone to process it**.

---

# 4️⃣ **Operator – Advanced Controller with Logic**

### **What is an Operator?**

An **Operator = CRD + Controller + Domain Knowledge**.

It automates operational tasks for an application:

* Upgrade
* Backup
* Failover
* Scaling
* Monitoring

Operators are usually built using tools like:

* **Operator SDK**
* **Kubebuilder**
* **Metacontroller**

### Think of it like:

**Operator = Human Operator + Kubernetes Automation**

Example:
**MongoDB Operator**
→ Watches MongoDB CR
→ Deploys Mongo
→ Creates replica sets
→ Automates failover
→ Automates scaling
→ Manages backups

---

# 🔥 Real-World Example — **MySQL Operator**

### Step 1: Create CRD

```yaml
kind: CustomResourceDefinition
metadata:
  name: mysqlclusters.mysql.example.com
spec:
  group: mysql.example.com
  versions:
  - name: v1
    served: true
    storage: true
  scope: Namespaced
  names:
    plural: mysqlclusters
    singular: mysqlcluster
    kind: MysqlCluster
```

### Step 2: Create a Custom Resource (CR)

```yaml
apiVersion: mysql.example.com/v1
kind: MysqlCluster
metadata:
  name: my-mysql
spec:
  replicas: 3
  version: "8.0"
```

### Step 3: Operator Logic (Controller actions)

* On `replicas: 3` → Create 3 MySQL Pods + Service
* On version upgrade → Rolling update of pods
* On delete → Clean up PVCs and Services
* On resource changes → Auto-tune MySQL config

---

# 5️⃣ **How They Work Together (Simple View)**

### 🔸 Step-by-step Flow

1. **CRD** defines new API (like MysqlCluster)
2. **User creates CR** (MysqlCluster instance)
3. **Controller watches CR** (detect new cluster request)
4. Controller creates resources:

   * Pods
   * Services
   * Volume
5. **Operator adds intelligence**

   * Upgrades
   * Backup
   * Recovery
   * Auto-scaling

---

# 6️⃣ Quick Summary (Interview-Ready)

### ✔ **CRD**

Defines the schema (new API type).

### ✔ **Custom Resource (CR)**

The actual object created by the user.

### ✔ **Controller**

A control loop that reconciles desired vs current state.

### ✔ **Operator**

A controller with domain-specific logic to automate application lifecycle.

---

# 7️⃣ When to Use What?

| Requirement                                                   | Use        |
| ------------------------------------------------------------- | ---------- |
| Want custom API?                                              | CRD        |
| Provide input configurations?                                 | CR         |
| Need automation to create resources?                          | Controller |
| Need production-level automation? (upgrade, backup, failover) | Operator   |

---

