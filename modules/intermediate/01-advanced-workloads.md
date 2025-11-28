# Module 07: Advanced Workloads

## Learning Objectives
By the end of this module, you will be able to:
- Manage stateful applications using StatefulSets (Assigned Seating).
- Run system daemons using DaemonSets (The Cleaning Crew).
- Run batch processes using Jobs and CronJobs (The Task List).

## 1. StatefulSets

Deployments are great for stateless apps (like web servers). But what about databases?
**Problem**: If you have a database cluster, "Replica 1" is usually the Master and "Replica 2" is the Slave. They are NOT interchangeable.
**Solution**: **StatefulSets**.

**Analogy**: **Assigned Seating** at a Wedding.
- **Deployment**: Free seating. Anyone can sit anywhere. If one guest leaves, another random guest takes their place.
- **StatefulSet**: Place cards. "Uncle Bob" must sit at "Table 1, Seat A". If Uncle Bob leaves, you don't put "Aunt Mary" there; you wait for "Uncle Bob" (or his clone) to come back.

### Key Features
- **Stable Network ID**: Pods get names like `web-0`, `web-1` (not random `web-x7z9`).
- **Stable Storage**: `web-0` always gets the same PVC. If `web-0` dies and restarts on a different node, it re-attaches to the *same* disk.
- **Ordered Deployment**: `web-1` won't start until `web-0` is ready.

### 1.1 Example StatefulSet
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  serviceName: "nginx"
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
```

## 2. DaemonSets

A **DaemonSet** ensures that all (or some) Nodes run a copy of a Pod.

**Analogy**: **The Cleaning Crew** or **Security Guards**.
- Every floor (Node) in the building needs exactly one security guard.
- If you build a new floor (Add a Node), the agency automatically sends a new guard.
- If you demolish a floor (Remove a Node), the guard goes home.

Typical uses:
- **Storage Daemons**: `glusterd`, `ceph`.
- **Log Collectors**: `fluentd` (to grab logs from every node).
- **Monitoring**: `node-exporter` (to check CPU/RAM of every node).

### 2.1 Example DaemonSet
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd-elasticsearch
spec:
  selector:
    matchLabels:
      name: fluentd-elasticsearch
  template:
    metadata:
      labels:
        name: fluentd-elasticsearch
    spec:
      containers:
      - name: fluentd-elasticsearch
        image: quay.io/fluentd_elasticsearch/fluentd:v2.5.2
```

## 3. Jobs and CronJobs

### 3.1 Jobs
A **Job** creates one or more Pods and ensures that a specified number of them successfully terminate.

**Analogy**: **A Contractor**.
- You hire a contractor to fix the roof.
- Once the roof is fixed (Job Complete), the contractor leaves.
- If the contractor gets sick (Pod fails), the agency sends another one until the job is done.

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  template:
    spec:
      containers:
      - name: pi
        image: perl
        command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never
```

### 3.2 CronJobs
A **CronJob** creates Jobs on a repeating schedule.

**Analogy**: **Scheduled Maintenance**.
- "Clean the windows every Monday at 9 AM."

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "*/1 * * * *" # Run every minute
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox
            imagePullPolicy: IfNotPresent
            command:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
```

## Summary
- **StatefulSet** (Assigned Seating): For databases and stateful apps.
- **DaemonSet** (Cleaning Crew): For system agents on every node.
- **Job** (Contractor): For run-to-completion tasks.
- **CronJob** (Maintenance Schedule): For scheduled tasks.

[Next: Module 08 - Scheduling & Resource Management](02-scheduling.md)
