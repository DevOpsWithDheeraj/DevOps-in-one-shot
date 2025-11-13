
# 🧭 **Kubernetes (k8s) All Commands**

## ⚙️ 1. **Cluster Information Commands**

Used to view cluster status and info.

| Command                             | Description                                                     | Example                                   |
| ----------------------------------- | --------------------------------------------------------------- | ----------------------------------------- |
| `kubectl version`                   | Shows client and server version                                 | `kubectl version --short`                 |
| `kubectl cluster-info`              | Displays cluster master and service endpoints                   | `kubectl cluster-info`                    |
| `kubectl get componentstatuses`     | Checks health of core components (deprecated in newer versions) | `kubectl get cs`                          |
| `kubectl config view`               | Shows kubeconfig details                                        | `kubectl config view`                     |
| `kubectl config get-contexts`       | Lists all contexts                                              | `kubectl config get-contexts`             |
| `kubectl config use-context <name>` | Switch between clusters                                         | `kubectl config use-context prod-cluster` |

---

## 🧩 2. **Get / List Resources**

Used to list Kubernetes objects.

| Command                   | Description                      | Example                          |
| ------------------------- | -------------------------------- | -------------------------------- |
| `kubectl get pods`        | Lists all pods                   | `kubectl get pods -n default`    |
| `kubectl get svc`         | Lists all services               | `kubectl get svc -n dev`         |
| `kubectl get deployments` | Lists all deployments            | `kubectl get deployments`        |
| `kubectl get nodes`       | Lists all nodes                  | `kubectl get nodes -o wide`      |
| `kubectl get all`         | Lists all objects in a namespace | `kubectl get all -n kube-system` |

---

## 🧱 3. **Create / Apply Resources**

Used to deploy new resources from YAML or commands.

| Command                   | Description                                  | Example                                              |
| ------------------------- | -------------------------------------------- | ---------------------------------------------------- |
| `kubectl create`          | Creates resources from command or file       | `kubectl create -f pod.yaml`                         |
| `kubectl apply -f <file>` | Applies configuration file (creates/updates) | `kubectl apply -f deployment.yaml`                   |
| `kubectl run`             | Run a single pod                             | `kubectl run nginx --image=nginx`                    |
| `kubectl expose`          | Expose a pod or deployment as a service      | `kubectl expose pod nginx --port=80 --type=NodePort` |

---

## 🧰 4. **Describe / Inspect Resources**

| Command                        | Description                       | Example                               |
| ------------------------------ | --------------------------------- | ------------------------------------- |
| `kubectl describe pod <name>`  | Detailed info about a pod         | `kubectl describe pod nginx-pod`      |
| `kubectl describe node <name>` | Node details                      | `kubectl describe node worker1`       |
| `kubectl explain <resource>`   | Documentation for a resource type | `kubectl explain pod.spec.containers` |

---

## 📦 5. **Delete Resources**

| Command                         | Description                       | Example                             |
| ------------------------------- | --------------------------------- | ----------------------------------- |
| `kubectl delete pod <name>`     | Delete a specific pod             | `kubectl delete pod nginx-pod`      |
| `kubectl delete -f <file>`      | Delete from YAML                  | `kubectl delete -f deployment.yaml` |
| `kubectl delete all --all`      | Delete all resources in namespace | `kubectl delete all --all -n dev`   |
| `kubectl delete ns <namespace>` | Delete a namespace                | `kubectl delete ns test`            |

---

## 🧭 6. **Namespace Management**

| Command                                                 | Description            | Example                                                |
| ------------------------------------------------------- | ---------------------- | ------------------------------------------------------ |
| `kubectl get ns`                                        | List namespaces        | `kubectl get ns`                                       |
| `kubectl create ns <name>`                              | Create a new namespace | `kubectl create ns dev`                                |
| `kubectl delete ns <name>`                              | Delete a namespace     | `kubectl delete ns test`                               |
| `kubectl config set-context --current --namespace=<ns>` | Set default namespace  | `kubectl config set-context --current --namespace=dev` |

---

## 🐳 7. **Pod Management Commands**

| Command                               | Description                     | Example                                  |
| ------------------------------------- | ------------------------------- | ---------------------------------------- |
| `kubectl get pods -o wide`            | View pods with node and IP info | `kubectl get pods -o wide`               |
| `kubectl logs <pod>`                  | View logs of a pod              | `kubectl logs nginx-pod`                 |
| `kubectl logs -f <pod>`               | Stream logs                     | `kubectl logs -f nginx-pod`              |
| `kubectl exec -it <pod> -- /bin/bash` | Access container shell          | `kubectl exec -it nginx-pod -- bash`     |
| `kubectl port-forward <pod> 8080:80`  | Port forward to pod             | `kubectl port-forward nginx-pod 8080:80` |
| `kubectl top pod`                     | Show CPU/memory usage           | `kubectl top pod nginx-pod`              |

---

## 🧱 8. **Deployment Management**

| Command                                            | Description              | Example                                              |
| -------------------------------------------------- | ------------------------ | ---------------------------------------------------- |
| `kubectl get deployments`                          | List deployments         | `kubectl get deployments`                            |
| `kubectl scale deployment <name> --replicas=<num>` | Scale deployment         | `kubectl scale deployment nginx-deploy --replicas=4` |
| `kubectl rollout status deployment/<name>`         | View rollout status      | `kubectl rollout status deployment/nginx-deploy`     |
| `kubectl rollout history deployment/<name>`        | Check deployment history | `kubectl rollout history deployment/nginx-deploy`    |
| `kubectl rollout undo deployment/<name>`           | Rollback deployment      | `kubectl rollout undo deployment/nginx-deploy`       |

---

## 🌐 9. **Service & Networking Commands**

| Command                                                | Description                     | Example                                                  |
| ------------------------------------------------------ | ------------------------------- | -------------------------------------------------------- |
| `kubectl get svc`                                      | List services                   | `kubectl get svc`                                        |
| `kubectl expose pod <pod> --type=NodePort --port=80`   | Expose a pod                    | `kubectl expose pod nginx-pod --type=NodePort --port=80` |
| `kubectl get endpoints`                                | Show endpoints behind a service | `kubectl get endpoints`                                  |
| `kubectl port-forward svc/<svc-name> <local>:<target>` | Forward port from service       | `kubectl port-forward svc/nginx-service 8080:80`         |

---

## 🧮 10. **ConfigMap & Secret Commands**

| Command                             | Description        | Example                                                                |
| ----------------------------------- | ------------------ | ---------------------------------------------------------------------- |
| `kubectl create configmap`          | Create configmap   | `kubectl create configmap app-config --from-literal=ENV=prod`          |
| `kubectl get configmap`             | List configmaps    | `kubectl get configmap`                                                |
| `kubectl describe configmap <name>` | Describe configmap | `kubectl describe configmap app-config`                                |
| `kubectl create secret generic`     | Create secret      | `kubectl create secret generic db-secret --from-literal=PASSWORD=1234` |
| `kubectl get secret`                | List secrets       | `kubectl get secret`                                                   |

---

## ⚙️ 11. **Autoscaling Commands**

| Command                               | Description                      | Example                                                               |
| ------------------------------------- | -------------------------------- | --------------------------------------------------------------------- |
| `kubectl autoscale deployment <name>` | Create Horizontal Pod Autoscaler | `kubectl autoscale deployment nginx --min=2 --max=5 --cpu-percent=70` |
| `kubectl get hpa`                     | List HPAs                        | `kubectl get hpa`                                                     |

---

## 🔁 12. **Resource Apply / Patch / Edit**

| Command                                     | Description             | Example                                                       |
| ------------------------------------------- | ----------------------- | ------------------------------------------------------------- |
| `kubectl edit <resource> <name>`            | Edit resource in editor | `kubectl edit deployment nginx`                               |
| `kubectl patch <resource> <name> -p <json>` | Patch a resource        | `kubectl patch deployment nginx -p '{"spec":{"replicas":3}}'` |
| `kubectl replace -f <file>`                 | Replace configuration   | `kubectl replace -f pod.yaml`                                 |

---

## 📊 13. **Monitoring & Logs**

| Command                             | Description                   | Example                             |
| ----------------------------------- | ----------------------------- | ----------------------------------- |
| `kubectl top nodes`                 | Show resource usage by nodes  | `kubectl top nodes`                 |
| `kubectl top pods`                  | Show pod-level metrics        | `kubectl top pods --all-namespaces` |
| `kubectl logs <pod>`                | View container logs           | `kubectl logs mypod`                |
| `kubectl logs <pod> -c <container>` | Logs for a specific container | `kubectl logs mypod -c nginx`       |

---

## 🧰 14. **Debugging & Troubleshooting**

| Command                          | Description                | Example                                                    |
| -------------------------------- | -------------------------- | ---------------------------------------------------------- |
| `kubectl describe pod <pod>`     | See events, errors         | `kubectl describe pod nginx`                               |
| `kubectl get events`             | View cluster events        | `kubectl get events --sort-by=.metadata.creationTimestamp` |
| `kubectl debug`                  | Create debugging container | `kubectl debug nginx-pod -it --image=busybox`              |
| `kubectl cp <pod>:/path ./local` | Copy from pod              | `kubectl cp nginx-pod:/var/log/nginx ./logs`               |

---

## 🧩 15. **Node Management**

| Command                        | Description             | Example                                     |
| ------------------------------ | ----------------------- | ------------------------------------------- |
| `kubectl get nodes`            | List nodes              | `kubectl get nodes`                         |
| `kubectl cordon <node>`        | Mark node unschedulable | `kubectl cordon worker1`                    |
| `kubectl uncordon <node>`      | Make node schedulable   | `kubectl uncordon worker1`                  |
| `kubectl drain <node>`         | Safely evict all pods   | `kubectl drain worker1 --ignore-daemonsets` |
| `kubectl describe node <node>` | Node details            | `kubectl describe node worker1`             |

---

## ⚡ 16. **Shortcuts & Formatting**

| Command                                                    | Description                     | Example                                                    |
| ---------------------------------------------------------- | ------------------------------- | ---------------------------------------------------------- |
| `kubectl get pods -o wide`                                 | Detailed pod info               | `kubectl get pods -o wide`                                 |
| `kubectl get pods -o yaml`                                 | Output in YAML                  | `kubectl get pods nginx -o yaml`                           |
| `kubectl get pods -o jsonpath='{.items[*].metadata.name}'` | Extract specific field          | `kubectl get pods -o jsonpath='{.items[*].metadata.name}'` |
| `kubectl get all --all-namespaces`                         | All resources in all namespaces | `kubectl get all --all-namespaces`                         |

---

## 🧠 Pro Tip

To quickly **learn all resource types and short forms:**

```bash
kubectl api-resources
```

→ Lists all resources (like `pods`, `deployments`, `svc`, etc.) and their short aliases (`po`, `deploy`, `svc`).

---

## 🧾 Summary Table of Most Used Commands

| Category | Common Command                              |
| -------- | ------------------------------------------- |
| Get Info | `kubectl get pods,svc,deploy`               |
| Create   | `kubectl apply -f file.yaml`                |
| Delete   | `kubectl delete -f file.yaml`               |
| Logs     | `kubectl logs pod-name`                     |
| Exec     | `kubectl exec -it pod-name -- bash`         |
| Scale    | `kubectl scale deployment app --replicas=3` |
| Rollback | `kubectl rollout undo deployment/app`       |

---
