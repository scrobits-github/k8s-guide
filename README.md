# From Zero to Kube-god

Welcome to your end‑to‑end, production‑grade Kubernetes tutorial. This curriculum is crafted for technical founders and staff‑plus engineers who already know the basics (Deployments, Services, Probes, Ingress, Helm, ArgoCD, Limits/Requests, HPA, CronJobs, Secrets/ConfigMaps, EKS & K3s) and are eager to dive into advanced internals, day‑2 operations, edge cases, security, performance, multi‑cluster, and cutting‑edge (v1.33) features.

## Curriculum Overview

| Mod | Theme | Key Topics |
|-----|-------|-----------|
| [01](modules/01-cluster-architecture-deep-dive.md) | **Cluster Architecture Deep‑Dive** | Control‑plane internals, etcd health, leader election, HA patterns |
| [02](modules/02-networking-beyond-services.md) | **Networking Beyond Services** | CNI (Calico, Cilium + eBPF), NetworkPolicy, Gateway API, multi‑NIC, IPv6/dual‑stack |
| [03](modules/03-storage-stateful-workloads.md) | **Storage & Stateful Workloads** | CSI drivers, dynamic provisioning, StatefulSets patterns, RWX at scale, Rook‑Ceph |
| [04](modules/04-advanced-scheduling.md) | **Advanced Scheduling** | Affinity/anti‑affinity, taints‑tolerations, topology‑spread, NUMA & GPUs, Karpenter, descheduler |
| [05](modules/05-security-fundamentals-2.0.md) | **Security Fundamentals 2.0** | RBAC design, Pod Security Standards, seccomp, AppArmor, SELinux, secrets‑management, sigstore/cosign |
| [06](modules/06-policy-governance.md) | **Policy & Governance** | OPA/Gatekeeper, Kyverno, admission webhooks, multi‑tenancy models |
| [07](modules/07-observability-debugging.md) | **Observability & Debugging** | Prometheus Operator, Grafana LGTM stack, Loki, Tempo, OpenTelemetry, ephemeral containers, eBPF profiling |
| [08](modules/08-autoscaling-revisited.md) | **Autoscaling Revisited** | VPA, KEDA, Karpenter, Event‑driven HPA, custom metrics adapters |
| [09](modules/09-service-mesh-api-gateway.md) | **Service Mesh & Beyond** | Istio Ambient, Linkerd 2, Cilium Service Mesh, Gateway API interplay, mTLS pitfalls |
| [10](modules/10-gitops-continuous-delivery.md) | **GitOps at Scale** | ArgoCD advanced sync‑waves, Flux v2, Progressive Delivery (canary, blue‑green), GitOps MTS pattern |
| [11](modules/11-operators-custom-controllers.md) | **Operators & Custom Controllers** | Kubebuilder, controller‑runtime, CRDs best‑practice, Operator Lifecycle Manager |
| [12](modules/12-multi-cluster-edge.md) | **Multi‑Cluster & Edge** | Cluster‑API, Submariner, KubeFed v2, KubeEdge, K3s on IoT gateways |
| [13](modules/13-cost-performance.md) | **Cost & Performance** | FinOps dashboards, right‑sizing, spot node‑pools, resource quotas, audit for orphaned PVs |
| [14](modules/14-day2-operations.md) | **Day‑2 Ops** | Zero‑downtime upgrades, surge vs partition, backup/restore (Velero & etcd‑snapshots), DR drills |
| [15](modules/15-emerging-features.md) | **Emerging Features (v1.33)** | Stable sidecar containers, mixed‑protocol Service, in‑place Pod updates, WASM runtimes, CRI v1, cluster‑reset API |

---

## ✨ Style & Execution Notes

- **Professional yet approachable** – Acronyms are explained on first use.
- **Stories & metaphors** are sprinkled in to engage leadership.
- **Mermaid** and **ASCII** diagrams are included for visual learners.
- YAML blocks are kept ≤ 80 columns, with inline comments (e.g. `# ⚠️ PodSecurity`) for clarity.
- Labs assume a **Kubernetes v1.33** (released April 23, 2025) API and run on an **8‑CPU / 16 GiB** (or smaller) laptop.
- **kind** (or **k3d**) is preferred; cloud specifics (EKS, AKS, GKE) are noted in call‑outs.
- Deprecated APIs (from 1.31 → 1.33) are flagged.

---

## 🧪 Hands‑on Labs

Each module's "Hands‑on lab" section is copy‑paste‑ready (using **kind** or **k3d**), with cloud‑specific notes for EKS, AKS, or GKE. (Helm charts are used where helpful.)

---

## 🚀 Further Reading

Every module ends with 3–5 links (official docs, KEPs, CNCF projects) so you can dive deeper.

---

## 🔚 Closing

From zero to Kube‑god – you're now armed with the advanced internals, day‑2 ops, edge cases, security, performance, multi‑cluster, and cutting‑edge (v1.33) features. Happy Kube‑ing! 