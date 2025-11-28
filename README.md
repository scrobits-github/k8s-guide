# From Zero to Kube-god

Welcome to your end‑to‑end, production‑grade Kubernetes tutorial. This curriculum is crafted to take you from **absolute zero** to **advanced internals**.

Whether you are a beginner just starting out or a staff‑plus engineer looking to dive into advanced internals, this guide has something for you.

## Curriculum Overview

### Part 1: Basics (Zero to Hero)
| Mod | Theme | Key Topics |
|-----|-------|-----------|
| [01](modules/basics/01-intro-and-architecture.md) | **Intro & Architecture** | What is K8s? Containers vs VMs, Control Plane vs Worker Nodes |
| [02](modules/basics/02-first-steps.md) | **First Steps** | kubectl, kind, Imperative vs Declarative, First Pod |
| [03](modules/basics/03-workloads.md) | **Workload Management** | ReplicaSets, Deployments, Rolling Updates, Namespaces |
| [04](modules/basics/04-networking-basics.md) | **Networking Basics** | Services (ClusterIP, NodePort, LoadBalancer), DNS, Ingress |
| [05](modules/basics/05-config-storage.md) | **Configuration** | ConfigMaps, Secrets, Env Vars vs Volumes |
| [06](modules/basics/06-storage-basics.md) | **Storage Basics** | PersistentVolumes (PV), PVCs, StorageClasses |

### Part 2: Intermediate (Day-to-Day Operations)
| Mod | Theme | Key Topics |
|-----|-------|-----------|
| [07](modules/intermediate/01-advanced-workloads.md) | **Advanced Workloads** | StatefulSets, DaemonSets, Jobs, CronJobs |
| [08](modules/intermediate/02-scheduling.md) | **Scheduling & Resources** | Requests/Limits, Labels/Selectors, Taints/Tolerations, Affinity |
| [09](modules/intermediate/03-security-basics.md) | **Security Basics** | ServiceAccounts, RBAC (Role, ClusterRole), SecurityContext |
| [10](modules/intermediate/04-observability.md) | **Observability** | Probes (Liveness/Readiness), Logging, Metrics Server |
| [11](modules/intermediate/05-helm.md) | **Helm & Packaging** | Helm Charts, Repositories, Creating Charts |

### Part 3: Advanced (Internals & Production)
| Mod | Theme | Key Topics |
|-----|-------|-----------|
| [12](modules/advanced/01-cluster-architecture-deep-dive.md) | **Cluster Architecture Deep‑Dive** | Control‑plane internals, etcd health, leader election, HA patterns |
| [13](modules/advanced/02-networking-beyond-services.md) | **Networking Beyond Services** | CNI (Calico, Cilium + eBPF), NetworkPolicy, Gateway API, multi‑NIC |
| [14](modules/advanced/03-storage-stateful-workloads.md) | **Storage & Stateful Workloads** | CSI drivers, dynamic provisioning, RWX at scale, Rook‑Ceph |
| [15](modules/advanced/04-advanced-scheduling.md) | **Advanced Scheduling** | Topology‑spread, NUMA & GPUs, Karpenter, descheduler |
| [16](modules/advanced/05-security-fundamentals-2.0.md) | **Security Fundamentals 2.0** | Pod Security Standards, seccomp, AppArmor, SELinux, sigstore/cosign |
| [17](modules/advanced/06-policy-governance.md) | **Policy & Governance** | OPA/Gatekeeper, Kyverno, admission webhooks |
| [18](modules/advanced/07-observability-debugging.md) | **Observability & Debugging** | Prometheus Operator, Grafana LGTM stack, OpenTelemetry, eBPF profiling |
| [19](modules/advanced/08-autoscaling-revisited.md) | **Autoscaling Revisited** | VPA, KEDA, Karpenter, Event‑driven HPA |
| [20](modules/advanced/09-service-mesh-api-gateway.md) | **Service Mesh & Beyond** | Istio Ambient, Linkerd 2, Cilium Service Mesh |
| [21](modules/advanced/10-gitops-continuous-delivery.md) | **GitOps at Scale** | ArgoCD, Flux v2, Progressive Delivery (canary, blue‑green) |
| [22](modules/advanced/11-operators-custom-controllers.md) | **Operators & Custom Controllers** | Kubebuilder, controller‑runtime, CRDs |
| [23](modules/advanced/12-multi-cluster-edge.md) | **Multi‑Cluster & Edge** | Cluster‑API, Submariner, KubeFed v2, KubeEdge |
| [24](modules/advanced/13-cost-performance.md) | **Cost & Performance** | FinOps dashboards, right‑sizing, spot node‑pools |
| [25](modules/advanced/14-day2-operations.md) | **Day‑2 Ops** | Zero‑downtime upgrades, backup/restore (Velero), DR drills |
| [26](modules/advanced/15-emerging-features.md) | **Emerging Features** | Sidecar containers, WASM runtimes, cluster‑reset API |

---

## ✨ Style & Execution Notes

- **Professional yet approachable** – Concepts are explained clearly.
- **Hands-on** – Each module includes examples you can run locally.
- **Tools** – We use **kind** for local clusters and **kubectl** for interaction.

---

## 🚀 Getting Started

Start with [Module 01: Introduction & Architecture](modules/basics/01-intro-and-architecture.md) and work your way up!

## 🔚 Closing

From zero to Kube‑god – you're now armed with everything from basic pods to advanced internals. Happy Kube‑ing!