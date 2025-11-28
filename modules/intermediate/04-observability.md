# Module 10: Observability

## Learning Objectives
By the end of this module, you will be able to:
- Configure Liveness, Readiness, and Startup probes (The Health Checks).
- Access and manage container logs (The Diary).
- Understand the basics of Kubernetes monitoring (The Dashboard).

## 1. Probes (Health Checks)

Kubernetes needs to know if your application is healthy. It uses "Probes" to ping your app.

### 1.1 Liveness Probe
**Analogy**: **The Zombie Check**.
- Kubernetes asks: "Are you alive?"
- If you don't answer, it assumes you are a zombie/frozen and shoots you (Restarts the container).
- **Use Case**: Deadlocks, infinite loops.

### 1.2 Readiness Probe
**Analogy**: **The Traffic Light**.
- Kubernetes asks: "Are you ready to take traffic?"
- If you say "No" (or don't answer), it turns the light Red (Removes IP from Service). It does NOT kill you; it just stops sending customers your way.
- **Use Case**: App is initializing, loading a large cache, or waiting for a database connection.

### 1.3 Startup Probe
**Analogy**: **Morning Coffee**.
- Kubernetes asks: "Are you awake yet?"
- It gives the app time to start up (drink coffee) before the Liveness Probe starts pestering it.
- **Use Case**: Slow starting legacy applications (e.g., Java apps that take 2 minutes to boot).

### 1.4 Example Configuration
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: probes-demo
spec:
  containers:
  - name: nginx
    image: nginx
    livenessProbe:
      httpGet:
        path: /
        port: 80
      initialDelaySeconds: 3
      periodSeconds: 3
    readinessProbe:
      httpGet:
        path: /
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 5
```

## 2. Container Logging

In Kubernetes, logs are the standard output (`stdout`) and standard error (`stderr`) streams of the container.

**Analogy**: **The Captain's Log / Diary**.
- The container writes everything it does to `stdout`.
- Kubernetes collects these pages and keeps them for you.

### 2.1 Viewing Logs
```bash
# Get logs for a pod
kubectl logs my-pod

# Follow logs (tail -f) - Watch the diary being written in real-time
kubectl logs -f my-pod

# Get logs for a specific container in a multi-container pod
kubectl logs my-pod -c my-container

# Get logs for a previously terminated container (crash debugging)
# "Show me the diary of the guy who died"
kubectl logs my-pod --previous
```

## 3. Monitoring Basics

Kubernetes does not have a built-in monitoring solution for long-term metrics storage, but it exposes metrics via the **Metrics Server**.

**Analogy**: **Car Dashboard**.
- **Metrics Server**: Speedometer & Fuel Gauge (Current state only).
- **Prometheus**: A Flight Recorder (Records history).

### 3.1 Metrics Server
Enables `kubectl top` and Horizontal Pod Autoscaling (HPA).

```bash
# Check node usage (How much gas is left?)
kubectl top nodes

# Check pod usage (How fast is the engine spinning?)
kubectl top pods
```

### 3.2 Prometheus & Grafana
The industry standard for Kubernetes monitoring.
- **Prometheus**: Scrapes metrics from applications and Kubernetes components.
- **Grafana**: Visualizes the data (Pretty charts).

## Summary
- **Liveness** (Zombie Check) restarts dead apps.
- **Readiness** (Traffic Light) controls traffic flow.
- **Startup** (Coffee) buys time for slow boots.
- **Logs** (Diary) help you debug.
- **Metrics** (Dashboard) show you performance.

[Next: Module 11 - Helm & Packaging](05-helm.md)
