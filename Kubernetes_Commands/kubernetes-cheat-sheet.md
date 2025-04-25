# Kubernetes Basic Commands Cheat Sheet

## 🔍 Cluster & Configuration

### Show client and server version
```bash
kubectl version
```

### Display cluster information
```bash
kubectl cluster-info
```

### Show kubeconfig settings
```bash
kubectl config view
```

### Get current context
```bash
kubectl config current-context
```

### Set a context
```bash
kubectl config use-context <context-name>
```

---

## 🖥️ Nodes

### List all nodes
```bash
kubectl get nodes
```

### Get detailed info about a node
```bash
kubectl describe node <node-name>
```

---

## 📦 Pods

### List all pods in all namespaces
```bash
kubectl get pods --all-namespaces
```

### List all pods in the current namespace
```bash
kubectl get pods
```

### Get detailed info about a pod
```bash
kubectl describe pod <pod-name>
```

### Watch Pod after terminating --> again creating by using replicaset and deployment
```bash
kubectl get pod -w
```

### Delete a pod
```bash
kubectl delete pod <pod-name>
```

### Execute a command inside a pod
```bash
kubectl exec -it <pod-name> -- /bin/bash
```

### Get logs from a pod
```bash
kubectl logs <pod-name>
```

### Port forwarding
```bash
kubectl port-forward <pod-name> <local-port>:<remote-port>
```

### Delete all pods in a namespace
```bash
kubectl delete pods --all -n <namespace>
```

---

## ⚙️ Deployments & Scaling

### Scale a deployment
```bash
kubectl scale deployment <deployment-name> --replicas=<number>
```

### Expose a pod or deployment
```bash
kubectl expose pod <pod-name> --type=NodePort --port=<port>
```

---

## 📂 Resources (Files)

### Apply a configuration from a file
```bash
kubectl apply -f <filename>.yaml
```

### Delete resources from a file
```bash
kubectl delete -f <filename>.yaml
```

### Create a resource
```bash
kubectl create -f <filename>.yaml
```

---

## 🌐 Services

### Get all services
```bash
kubectl get svc
```

---

## 📊 Monitoring

### Get events
```bash
kubectl get events
```

### Get resource usage (requires Metrics Server)
```bash
kubectl top pod
kubectl top node
```

---

## 🛠️ Shortcuts

```bash
kubectl get po         # get pods
kubectl get svc        # get services
kubectl get deploy     # get deployments
kubectl get ns         # get namespaces
```
