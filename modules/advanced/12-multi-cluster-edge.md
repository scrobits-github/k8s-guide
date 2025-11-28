# Module 12: Multi-Cluster & Edge

## Why it matters

A global logistics company needed to run Kubernetes clusters in dozens of warehouses and across multiple clouds. They faced problems with network partitioning, inconsistent policy, and edge device outages. By adopting Cluster-API for fleet management, Submariner for cross-cluster networking, and KubeEdge for IoT, they achieved seamless workload mobility and resilient edge operations. This module shows how to master multi-cluster and edge patterns.

## Core concepts

- **Cluster-API**: Declarative cluster lifecycle management for any infrastructure (cloud, bare-metal, edge).
- **Submariner**: Enables direct pod-to-pod connectivity and service discovery across clusters, even behind NAT.
- **KubeFed v2**: Federates resources and policies across clusters for global consistency.
- **KubeEdge**: Extends Kubernetes to edge/IoT devices, supporting offline operation and device management.
- **K3s on IoT**: Lightweight Kubernetes for ARM/edge gateways, with reduced resource footprint.

## Hands-on lab

```bash
# Prereqs: kind, k3d, kubectl, clusterctl, helm, submariner, kubeedge

# 1. Create two clusters (one cloud, one edge)
kind create cluster --name cloud-cluster
k3d cluster create edge-cluster --servers 1 --agents 1

# 2. Install Cluster-API on the management cluster
clusterctl init --infrastructure=docker

# 3. Register both clusters with Cluster-API
# (see docs for cloud provider specifics)

# 4. Install Submariner for cross-cluster networking
helm repo add submariner-latest https://submariner-io.github.io/submariner-charts
helm install submariner submariner-latest/submariner \
  --set globalnet.enabled=true \
  --set broker.enabled=true \
  --namespace submariner-operator --create-namespace

# 5. Join edge cluster to Submariner broker
# (repeat helm install with --set broker.enabled=false and join token)

# 6. Enable KubeFed v2 for resource federation
kubectl create ns kube-federation-system
helm repo add kubefed-charts https://kubefed.github.io/helm-charts
helm install kubefed kubefed-charts/kubefed \
  --namespace kube-federation-system

# 7. Federate a namespace and deployment
kubefedctl join cloud-cluster --host-cluster-context=kind-cloud-cluster
kubefedctl join edge-cluster --host-cluster-context=k3d-edge-cluster

cat <<EOF | kubectl apply -f -
apiVersion: types.kubefed.io/v1beta1
kind: FederatedNamespace
metadata:
  name: global-app
spec:
  placement:
    clusters:
    - name: cloud-cluster
    - name: edge-cluster
EOF

# 8. Deploy KubeEdge on the edge cluster
helm repo add kubeedge https://kubeedge.github.io/kubeedge
helm install kubeedge kubeedge/kubeedge \
  --namespace kubeedge-system --create-namespace

# 9. Deploy a sample IoT workload on K3s
cat <<EOF | kubectl apply -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iot-sensor
  namespace: global-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: iot-sensor
  template:
    metadata:
      labels:
        app: iot-sensor
    spec:
      containers:
      - name: sensor
        image: ghcr.io/your-org/iot-sensor:latest
        resources:
          requests:
            cpu: 50m
            memory: 64Mi
EOF

# 10. Test cross-cluster connectivity
kubectl exec -n global-app deploy/iot-sensor -- curl http://service.global-app.svc.clusterset.local

# Cloud note: Submariner supports EKS, AKS, GKE, and on-prem clusters.
```

## Diagrams

```mermaid
graph TD
    subgraph "Management"
        CAP[Cluster-API]
        Fed[KubeFed v2]
    end
    subgraph "Cloud Cluster"
        CC[Cloud Cluster]
        SubC[Submariner]
    end
    subgraph "Edge Cluster"
        EC[Edge Cluster (K3s)]
        SubE[Submariner]
        KE[KubeEdge]
    end
    CAP --> CC
    CAP --> EC
    Fed --> CC
    Fed --> EC
    SubC <--> SubE
    EC --> KE
```

## Gotchas & troubleshooting

- **Submariner connectivity**
  - Check `subctl show all` for status and errors
  - Validate pod IPs with `kubectl get pods -o wide`
- **KubeFed sync issues**
  - Use `kubefedctl get federatednamespace` and check placement
  - Inspect controller logs for reconciliation errors
- **KubeEdge device disconnects**
  - Check `kubectl get devices` and edge node status
  - Use `kubectl logs -n kubeedge-system` for troubleshooting
- **K3s resource limits**
  - Monitor with `kubectl top nodes` and adjust workloads
- **Network partitioning**
  - Test failover by disconnecting edge cluster network

## Further reading

- [Cluster-API Docs](https://cluster-api.sigs.k8s.io/)
- [Submariner Project](https://submariner.io/)
- [KubeFed v2](https://github.com/kubernetes-sigs/kubefed)
- [KubeEdge Docs](https://kubeedge.io/)
- [KEP-1645: Multi-Cluster Services](https://github.com/kubernetes/enhancements/tree/master/keps/sig-multicluster/1645-multi-cluster-services) 