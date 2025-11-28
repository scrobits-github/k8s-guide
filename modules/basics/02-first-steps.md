# Module 02: First Steps

## Learning Objectives
By the end of this module, you will be able to:
- Install and configure `kubectl` (your command center).
- Set up a local Kubernetes cluster using `kind`.
- Understand the difference between Imperative (giving orders) and Declarative (describing the result).
- Run your first Pod and understand why it's called a "Pod".

## 1. Setting up the Environment

### 1.1 Install kubectl
`kubectl` (pronounced "kube-c-t-l", "kube-control", or "kube-cuddle") is the command-line tool for interacting with the Kubernetes API.

**Analogy**: Think of `kubectl` as the **remote control** for your drone (the cluster). You press buttons (commands) on the remote, and the drone executes them.

**MacOS (Homebrew)**:
```bash
brew install kubectl
```

**Verify installation**:
```bash
kubectl version --client
```

### 1.2 Install kind (Kubernetes in Docker)
`kind` lets you run Kubernetes clusters locally using Docker containers. It simulates a cluster inside your laptop.

**MacOS (Homebrew)**:
```bash
brew install kind
```

**Create a cluster**:
```bash
kind create cluster --name my-first-cluster
```

**Verify cluster**:
```bash
kubectl cluster-info
kubectl get nodes
```

## 2. Imperative vs. Declarative

There are two ways to tell Kubernetes what to do. Understanding the difference is crucial for becoming a "Kube-god".

### 2.1 Imperative (Do this now)
You give specific, step-by-step commands.
**Analogy**: A taxi driver. "Turn left, then go straight, then stop here."
**Pros**: Fast, good for quick tests.
**Cons**: Hard to reproduce, no history of what changed.

```bash
# Run an nginx pod
kubectl run nginx --image=nginx
```

### 2.2 Declarative (Make it look like this)
You define the *desired state* in a file and hand it to Kubernetes. Kubernetes figures out how to get there.
**Analogy**: A GPS navigation system. "I want to go to 123 Main St." The GPS figures out the turns. If you take a wrong turn, it recalculates to get you back on track.
**Pros**: Reproducible, version-controlled, self-healing.

```bash
# Apply a configuration
kubectl apply -f pod.yaml
```

## 3. Your First Pod

A **Pod** is the smallest deployable unit in Kubernetes.

### Why "Pod"?
Think of a **Pod of Whales** or a **Pea Pod**.
- A pea pod wraps one or more peas (containers).
- The peas inside share the same environment (water, nutrients).
- In Kubernetes, containers in a Pod share the same **IP address** and **Storage Volumes**.

**Key Concept**: You rarely run a container directly in Kubernetes; you wrap it in a Pod.

### 3.1 Create a Pod YAML
Create a file named `pod.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-first-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

### 3.2 Apply the Pod
```bash
kubectl apply -f pod.yaml
```

### 3.3 Inspect the Pod
```bash
# List pods
kubectl get pods

# Get detailed info (Events, IP, Node assignment)
kubectl describe pod my-first-pod
```

### 3.4 Delete the Pod
```bash
kubectl delete pod my-first-pod
# OR
kubectl delete -f pod.yaml
```

## Summary
- **kubectl** is your remote control.
- **Declarative** (GPS) is better than **Imperative** (Taxi Driver) for production.
- **Pod** is a wrapper around containers, like a pea pod holds peas. They share networking and storage.

[Next: Module 03 - Workload Management](03-workloads.md)
