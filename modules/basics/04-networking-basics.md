# Module 04: Networking Basics

## Learning Objectives
By the end of this module, you will be able to:
- Expose your applications using Services (The Receptionist).
- Understand the different Service types.
- Explain how Service Discovery works (Internal Phonebook).
- Use Ingress to route external traffic (The Smart Router).

## 1. Services

Pods are ephemeral (they die and get replaced). Their IP addresses change constantly.
**Problem**: How do you talk to a backend if its IP keeps changing?
**Solution**: A **Service**.

**Analogy**: Think of a **Company Receptionist**.
- You want to talk to "Customer Support" (The Pods).
- You don't call Bob's personal cell phone (Pod IP) because Bob might be on vacation or fired.
- You call the main company number (Service IP).
- The Receptionist (Service) forwards your call to *any* available support agent.

### 1.1 Service Types

#### ClusterIP (Default)
Exposes the Service on a cluster-internal IP.
**Analogy**: An **Internal Extension** (e.g., ext 101). Only people inside the office building (Cluster) can call it.

#### NodePort
Exposes the Service on each Node's IP at a static port.
**Analogy**: A **Side Door** to the building. Anyone who knows the door number (Port 30007) can enter from the street.

#### LoadBalancer
Exposes the Service externally using a cloud provider's load balancer.
**Analogy**: A **Public 1-800 Number**. It routes calls from the outside world to your internal receptionists.

### 1.2 Creating a Service
Create `service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector: # This is how the receptionist finds the workers
    app: nginx
  ports:
    - protocol: TCP
      port: 80 # The port the Service listens on
      targetPort: 80 # The port the Pod is listening on
  type: NodePort
```

**Apply it**:
```bash
kubectl apply -f service.yaml
```

**Access it**:
```bash
# Get the port
kubectl get svc nginx-service
# Access via localhost (if using kind with port mapping, or use port-forward)
kubectl port-forward svc/nginx-service 8080:80
```

## 2. Service Discovery (DNS)

Kubernetes has a built-in DNS server (CoreDNS). Services get DNS names.

**Analogy**: The **Company Directory**.
Instead of memorizing IP addresses (Phone numbers), you just look up "my-db" in the directory.

If you have a service named `my-db` in namespace `default`, other pods can reach it at:
`my-db.default.svc.cluster.local`

Or simply `my-db` if they are in the same namespace.

## 3. Ingress

**Ingress** exposes HTTP and HTTPS routes from outside the cluster to services within the cluster.

**Analogy**: An **Airport Traffic Control** or a **Hotel Concierge**.
- It sits at the front door.
- It looks at the request: "Oh, you want `example.com/blog`? Go to the Blog Service."
- "You want `example.com/shop`? Go to the Shop Service."

### 3.1 Ingress Resource Example

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: minimal-ingress
spec:
  rules:
  - http:
      paths:
      - path: /blog
        pathType: Prefix
        backend:
          service:
            name: blog-service
            port:
              number: 80
      - path: /shop
        pathType: Prefix
        backend:
          service:
            name: shop-service
            port:
              number: 80
```

> [!NOTE]
> To use Ingress, you must have an **Ingress Controller** (like Nginx Ingress Controller) running in your cluster. Think of the Ingress Resource as the "Rule Book" and the Ingress Controller as the "Traffic Cop" enforcing the rules.

## Summary
- **Services** (Receptionist) provide stable access to dynamic Pods.
- **ClusterIP** (Internal Ext) is for internal use.
- **NodePort** (Side Door) and **LoadBalancer** (1-800 Num) are for external access.
- **DNS** (Phonebook) lets services find each other by name.
- **Ingress** (Concierge) routes HTTP traffic based on URL.

[Next: Module 05 - Configuration](05-config-storage.md)
