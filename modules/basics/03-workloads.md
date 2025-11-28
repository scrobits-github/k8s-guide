# Module 03: Workload Management

## Learning Objectives
By the end of this module, you will be able to:
- Understand the role of ReplicaSets (The Crew).
- Deploy applications using Deployments (The Manager).
- Perform rolling updates and rollbacks (Zero Downtime).
- Organize resources using Namespaces (Virtual Clusters).

## 1. ReplicaSets

A **ReplicaSet** ensures that a specified number of Pod replicas are running at any given time.

**Analogy**: Think of a **Thermostat**.
- You set the temperature to 72°F (Desired State: 3 replicas).
- If it drops to 70°F (1 pod dies), the heater kicks on (ReplicaSet creates a pod).
- If it rises to 75°F (someone manually created extra pods), the AC kicks on (ReplicaSet deletes pods).

> [!NOTE]
> You rarely create ReplicaSets directly. You use **Deployments** instead, which manage ReplicaSets for you.

## 2. Deployments

A **Deployment** provides declarative updates for Pods and ReplicaSets. It is the standard way to run stateless applications.

**Analogy**:
- **Pod**: The Worker (does the job).
- **ReplicaSet**: The Team Lead (ensures enough workers are present).
- **Deployment**: The Project Manager (manages releases, updates, and versions).

You talk to the Project Manager (Deployment), who instructs the Team Lead (ReplicaSet), who hires/fires Workers (Pods).

### 2.1 Creating a Deployment
Create `deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3 # Desired state: 3 workers
  selector:
    matchLabels:
      app: nginx
  template: # The blueprint for the worker
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

**Apply it**:
```bash
kubectl apply -f deployment.yaml
```

**Verify**:
```bash
kubectl get deployments
kubectl get pods
```

### 2.2 Rolling Updates
Update the image version in your `deployment.yaml` to `nginx:1.16.1` and apply it again.

**What happens?**
The Deployment creates a **new** ReplicaSet and slowly scales it up, while scaling down the **old** ReplicaSet.
**Analogy**: Replacing a team of workers one by one so work never stops.

```bash
kubectl apply -f deployment.yaml
```

Watch the rollout happen:
```bash
kubectl rollout status deployment/nginx-deployment
```

### 2.3 Rollbacks
Made a mistake? The new version crashes? Roll back to the previous version.

```bash
kubectl rollout undo deployment/nginx-deployment
```

## 3. Namespaces

**Namespaces** provide a mechanism for isolating groups of resources within a single cluster.

**Analogy**: Think of a **Hard Drive**.
- The Cluster is the Hard Drive.
- Namespaces are **Folders** (e.g., `Work`, `Personal`, `Games`).
- You can have a file named `report.txt` in `Work` and another `report.txt` in `Personal`. They don't conflict.

### 3.1 Common Namespaces
- `default`: The default folder.
- `kube-system`: The system folder (Do not touch!).

### 3.2 Working with Namespaces
```bash
# Create a namespace
kubectl create namespace dev

# Run a pod in the 'dev' namespace
kubectl run nginx --image=nginx -n dev

# List pods in 'dev'
kubectl get pods -n dev
```

## Summary
- **ReplicaSets** (Thermostat) keep the right number of Pods running.
- **Deployments** (Project Manager) manage updates and versions.
- **Rolling Updates** allow you to update software without downtime.
- **Namespaces** (Folders) organize and isolate resources.

[Next: Module 04 - Networking Basics](04-networking-basics.md)
