

# 🚀 **Kubernetes Networking**
In Kubernetes, **Networking** refers to the **communication framework that enables Pods, Services, and external clients to communicate with each other**. It’s a critical part of Kubernetes because containers are ephemeral and can move across nodes, yet they need a reliable way to talk to each other.

**Kubernetes Networking** is the **set of rules, protocols, and components that manage how network traffic flows inside and outside the cluster**.

It ensures:
1. **Pod-to-Pod communication** across nodes without NAT.
2. **Pod-to-Service communication** via stable IPs.
3. **External access** through Services like NodePort, LoadBalancer, and Ingress.
4. **Network isolation** and security using Network Policies.

### **Key Principles:**

1. **Every Pod gets a unique IP address.**
   * Pods can communicate directly using their IP.
2. **Pods can communicate with all other Pods** by default, across nodes, without NAT.
3. **Services provide stable endpoints** for Pods that may change IPs.
4. **Network Policies can restrict traffic** to enforce security rules.

### **Components of Kubernetes Networking:**

| Component              | Role                                                       |
| ---------------------- | ---------------------------------------------------------- |
| **Pod Networking**     | Assigns IPs to Pods, allows Pod-to-Pod communication.      |
| **Service Networking** | Provides stable endpoints (ClusterIP) and load balancing.  |
| **Ingress**            | Manages external HTTP/HTTPS access and routing.            |
| **CNI Plugins**        | Implements the actual networking (Calico, Flannel, Weave). |
| **Network Policies**   | Define rules to allow or block traffic between Pods.       |

💡 **Tip:** You can think of Kubernetes networking as a **well-organized city**:
* Pods are houses with unique addresses (IPs).
* Services are apartment complexes with a main entrance (stable IP).
* Ingress is the city gate controlling who can enter.
* Network Policies are traffic rules to ensure safety.

---

# 1️⃣ **Services**
In Kubernetes, a **Service** is an **abstraction that defines a logical set of Pods and a policy to access them**.

A **Service** provides a **stable IP address and DNS name** to a group of Pods, even though the underlying Pods may change (due to scaling, failures, or updates).

>  **Purpose:** Allows communication between Pods or from external clients without worrying about the dynamic Pod IPs.

### **Key Features**

* **Stable endpoint:** Pods can come and go, but the Service provides a consistent IP/DNS.
* **Load balancing:** Distributes traffic evenly among Pods that match the Service selector.
* **Discovery:** Other Pods can discover a Service using its DNS name.
* **Decoupling:** Consumers of the Service don’t need to know about the specific Pods.

### **How it works**

* You define a **selector** in the Service that matches a set of Pods (usually via labels).
* Kubernetes automatically forwards traffic sent to the Service IP to the matching Pods.
* The Service type determines **how it is exposed** (ClusterIP, NodePort, LoadBalancer, or ExternalName).

### **4. Example YAML**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: ClusterIP
```

* **`selector`** → selects Pods with label `app: my-app`.
* **`port`** → port exposed by the Service.
* **`targetPort`** → port on the Pod receiving traffic.
* **`type`** → determines how the Service is exposed.

💡 **Summary:**
> A **Service** is like a **stable front door** to a set of Pods. Even if the Pods behind it change, the Service IP/DNS remains the same.
---

# 🔹 1. ClusterIP (Default Service)

In Kubernetes, ClusterIP is one of the Service types used to expose a set of Pods internally within the cluster. It is the default Service type. 

A **ClusterIP service** provides a **virtual IP address** (called **ClusterIP**) inside the Kubernetes cluster. This IP is only reachable **within the cluster**, not from the outside world.

> **Purpose:** Allows Pods and other services inside the cluster to communicate with each other using a stable IP, even if the Pods themselves change.


## **Example YAML**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: ClusterIP
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80       # Service port
      targetPort: 8080 # Pod port
```

* **`selector`** → chooses the Pods this service will route traffic to.
* **`port`** → the port on which the service is exposed internally.
* **`targetPort`** → the port on the Pod receiving traffic.
* **`type: ClusterIP`** → makes it accessible only inside the cluster.

💡 **Tip:** If you want the service to be accessible from **outside the cluster**, you would use `NodePort` or `LoadBalancer` instead of `ClusterIP`.

---

# 🔹 2. NodePort Service

In Kubernetes, **NodePort** is another **Service type** that allows you to expose your application **outside the cluster**. 

A **NodePort service** exposes a service on a **static port on each Node** of the Kubernetes cluster. This allows external clients to access your service using:

```
<NodeIP>:<NodePort>
```

* **NodeIP** → IP of any node in the cluster
* **NodePort** → port assigned by Kubernetes (default range: **30000–32767**) or manually defined.

## **How it works**

1. Kubernetes assigns a **ClusterIP** (internal IP) to the service, just like ClusterIP.
2. It also allocates a **NodePort**.
3. Traffic coming to `<NodeIP>:<NodePort>` is forwarded to the **ClusterIP**, which then routes it to the matching Pods.

This means NodePort is basically **ClusterIP + external exposure via node port**.

## **Example YAML**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport-service
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80         # Service port
      targetPort: 8080 # Pod port
      nodePort: 30080  # Optional: static NodePort, else Kubernetes assigns one
```

* **`port`** → internal cluster port
* **`targetPort`** → pod port receiving traffic
* **`nodePort`** → port on all cluster nodes exposed externally
* **`type: NodePort`** → enables access from outside

💡 **Tip:** NodePort is simple but not ideal for production in large clusters, because exposing many NodePorts can get messy. For production, **LoadBalancer** is preferred.

---

# 🔹 3. LoadBalancer Service

In Kubernetes, a **LoadBalancer** is a **Service type** that exposes your application to the **external world** with the help of a cloud provider's load balancer. 

A **LoadBalancer service** creates an **external IP** that routes traffic to your Kubernetes service.

* It uses the cloud provider’s (AWS, GCP, Azure) native load balancer.
* Internally, it still uses a **ClusterIP**, so traffic is routed to Pods via the cluster network.

### **How it works**

1. You create a Service with `type: LoadBalancer`.
2. Kubernetes requests the cloud provider to provision a **load balancer**.
3. The cloud load balancer receives traffic on a public IP and forwards it to the **NodePort**, which then forwards to the **ClusterIP**, and finally to the **Pods**.

### **Example YAML**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-loadbalancer-service
spec:
  type: LoadBalancer
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80         # External port
      targetPort: 8080 # Pod port
```

* **`port`** → port exposed externally (via LoadBalancer)
* **`targetPort`** → port on the Pod
* **`type: LoadBalancer`** → triggers external load balancer creation

💡 **Extra Tip:**

* In **bare-metal clusters** (no cloud), LoadBalancer won’t work out-of-the-box; you’d need solutions like **MetalLB** to simulate this functionality.
---

# 2️⃣ **Ingress**
In Kubernetes, **Ingress** is a resource that manages **external access to services** in a cluster, typically HTTP and HTTPS traffic. It provides **routing rules**, **load balancing**, and **SSL termination** for multiple services using a single entry point.

**Ingress** is a **Kubernetes object** that allows you to define how external traffic should reach your cluster services.

* Unlike NodePort or LoadBalancer, which expose one service per port, **Ingress can route multiple URLs or domains to different services**.
* Requires an **Ingress Controller** (like NGINX, Traefik) to implement the routing rules.

### **Key Features**

* **Path-based routing:** `/app1` → Service A, `/app2` → Service B
* **Host-based routing:** `app1.example.com` → Service A, `app2.example.com` → Service B
* **SSL/TLS termination:** Can manage HTTPS certificates
* **Load balancing:** Distributes traffic among Pods of a service

### **Example YAML**

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
    - host: myapp.example.com
      http:
        paths:
          - path: /app1
            pathType: Prefix
            backend:
              service:
                name: service-a
                port:
                  number: 80
          - path: /app2
            pathType: Prefix
            backend:
              service:
                name: service-b
                port:
                  number: 80
```

* **`host`** → domain name for routing
* **`path`** → URL path to route traffic
* **`backend`** → Service and port to send traffic to

💡 **Tip:** Think of Ingress as a **smart receptionist** at the front of your cluster: it checks the incoming request and decides **which service to send it to** based on URL or host.

---

# 3️⃣ **CNI – Container Network Interface**

In Kubernetes, **CNI** stands for **Container Network Interface**. It’s a **standard for configuring network interfaces in Linux containers**, and Kubernetes uses it to manage **networking between Pods**.

**CNI** is a **plugin-based networking specification** that allows Kubernetes Pods to communicate with each other and the outside world.

* It defines **how a Pod gets an IP address, how it connects to the network, and how traffic is routed**.
* Kubernetes itself doesn’t implement networking; it relies on **CNI plugins** like Calico, Flannel, or Weave Net.

### **Key Features**

* **Pod-to-Pod communication:** Ensures every Pod can communicate with any other Pod in the cluster.
* **IP management:** Assigns unique IP addresses to Pods.
* **Network policies:** Some CNI plugins allow restricting traffic between Pods.
* **Extensibility:** You can choose different CNI plugins based on your needs (performance, security, policy).

### **How it works**

1. When a Pod is created, Kubernetes calls the CNI plugin.
2. The plugin sets up the **network namespace** for the Pod.
3. It assigns an **IP address** and configures routing so the Pod can communicate with other Pods and services.

### **4. Popular CNI Plugins**

| Plugin        | Features                                         |
| ------------- | ------------------------------------------------ |
| **Calico**    | Network policies, high performance, supports BGP |
| **Flannel**   | Simple overlay network, easy to set up           |
| **Weave Net** | Automatic mesh network, encryption support       |
| **Cilium**    | Advanced network security, eBPF-based            |

💡 **Tip:** Think of CNI as the **network manager** of Kubernetes Pods. Without it, Pods wouldn’t be able to talk to each other or the outside world.

---

# 4️⃣ **Network Policies – Kubernetes Firewall**
In Kubernetes, **Network Policies** are used to **control network traffic between Pods**. They act like a **firewall for your cluster**, defining which Pods can communicate with each other and with external endpoints.

A **NetworkPolicy** is a **Kubernetes resource** that specifies **ingress (incoming) and egress (outgoing) rules** for Pods.

* By default, in Kubernetes **all Pods can communicate with all other Pods**.
* NetworkPolicies let you **restrict or allow traffic** based on **Pod selectors, namespaces, or IP blocks**.
* Requires a **CNI plugin that supports NetworkPolicies** (e.g., Calico, Cilium).

### **Key Features**

* **Pod-level control:** Apply rules to specific Pods using labels.
* **Namespace-level control:** Restrict traffic between namespaces.
* **Ingress & Egress rules:** Control incoming and outgoing traffic separately.
* **Security:** Helps implement zero-trust networking inside the cluster.

### **Example YAML**

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: backend
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              role: frontend
      ports:
        - protocol: TCP
          port: 8080
```

**Explanation:**

* Applies to Pods with label `role: backend`.
* Only allows **incoming traffic** from Pods labeled `role: frontend` on TCP port 8080.
* All other traffic to backend Pods is **blocked**.

💡 **Tip:** Without NetworkPolicies, Kubernetes networking is **open by default**. Think of NetworkPolicies as **rules for who can talk to whom** inside your cluster.

---

# 🧠 **Final Summary Table**

| Component         | Layer | Purpose                    |
| ----------------- | ----- | -------------------------- |
| **ClusterIP**     | L4    | Internal-only service      |
| **NodePort**      | L4    | Expose service via node IP |
| **LoadBalancer**  | L4    | External cloud LB          |
| **Ingress**       | L7    | Domain/path routing        |
| **CNI Plugin**    | L2/L3 | Pod networking, routing    |
| **NetworkPolicy** | L3/L4 | Pod-level firewall         |

---
