# Module 08: Scheduling & Resource Management

## Learning Objectives
By the end of this module, you will be able to:
- Define resource requests and limits for your containers (The Budget).
- Control Pod placement using Labels, Selectors, and Affinity (The Magnet).
- Restrict Pod placement using Taints and Tolerations (The Repellent).

## 1. Resource Requests and Limits

To ensure your cluster remains stable, you should specify how much CPU and Memory a container needs.

**Analogy**: **Salary Negotiation**.
- **Requests**: The **Minimum Salary** you need to accept the job. The Scheduler (Hiring Manager) won't hire you (schedule the pod) if they can't afford your minimum.
- **Limits**: The **Company Credit Card Limit**. You can spend up to this amount, but if you try to spend more, the transaction is declined (OOMKilled / Throttled).

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: resource-pod
spec:
  containers:
  - name: app
    image: nginx
    resources:
      requests:
        memory: "64Mi" # I need at least 64Mi to start
        cpu: "250m"    # I need 1/4 of a CPU core
      limits:
        memory: "128Mi" # If I use more than this, kill me
        cpu: "500m"     # If I try to use more, slow me down
```

## 2. Labels and Selectors

**Labels** are key/value pairs attached to objects.
**Selectors** allow you to filter objects based on labels.

This is the core mechanism for grouping in Kubernetes. It's like **Hashtags** on social media. You search for `#cats` to find all cat pictures.

## 3. Controlling Pod Placement

### 3.1 NodeSelector (Simple)
Assign a Pod to a node with a specific label.

```yaml
spec:
  nodeSelector:
    disktype: ssd # Only run on nodes with label disktype=ssd
```

### 3.2 Node Affinity (Advanced)
More expressive than NodeSelector.

**Analogy**: **Magnets**.
- **Required**: A strong magnet. "I MUST stick to metal."
- **Preferred**: A weak magnet. "I'd prefer to stick to metal, but if there is none, I'll stick to wood."

```yaml
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: disktype
            operator: In
            values:
            - ssd
```

## 4. Taints and Tolerations

**Taints** allow a Node to repel a set of Pods.
**Tolerations** allow a Pod to be scheduled on a tainted Node.

**Analogy**: **Bug Spray**.
1. **Taint**: You spray "Bug Repellent" on a Node.
2. **Normal Pods**: Like normal bugs, they hate the smell and won't land there.
3. **Toleration**: A "Super Bug" (Pod) wearing a gas mask. It tolerates the smell and can land there.

**Use Case**: Dedicated Nodes. Taint a node `gpu=true:NoSchedule`. Only Pods that *need* GPUs (and have the toleration) will schedule there.

**Pod Toleration**:
```yaml
spec:
  tolerations:
  - key: "key"
    operator: "Equal"
    value: "value"
    effect: "NoSchedule"
```

## Summary
- **Requests** (Minimum Salary) guarantee resources; **Limits** (Spending Cap) cap them.
- **Affinity** (Magnets) attracts Pods to Nodes.
- **Taints** (Bug Spray) repel Pods from Nodes.

[Next: Module 09 - Security Basics](03-security-basics.md)
