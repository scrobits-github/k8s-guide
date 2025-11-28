# Module 09: Security Basics

## Learning Objectives
By the end of this module, you will be able to:
- Understand Authentication (AuthN) vs Authorization (AuthZ).
- Manage permissions using RBAC (The Key Card System).
- Configure Pod security using SecurityContext (Privilege Level).

## 1. ServiceAccounts

Users (humans) have accounts managed externally (e.g., Google Accounts, LDAP).
**ServiceAccounts** are for processes (Pods) running in your cluster.

**Analogy**: **Robot Identity**.
- **User Account**: A Passport for a Human.
- **ServiceAccount**: A Serial Number for a Robot.
- When a Pod talks to the API Server, it shows its "Serial Number" (ServiceAccount Token) to prove who it is.

Every Pod runs as a ServiceAccount. If you don't specify one, it uses the `default` ServiceAccount in the namespace.

```bash
kubectl create serviceaccount my-app-sa
```

## 2. RBAC (Role-Based Access Control)

RBAC determines *who* (Subject) can do *what* (Verbs) to *which* resources (Objects).

**Analogy**: **Office Key Card System**.
- **Role**: A definition of access. e.g., "Can open the Server Room door".
- **RoleBinding**: Assigning that access to a person. e.g., "Give Bob the 'Can open Server Room' access".

### 2.1 Role (Namespaced)
Defines permissions within a specific namespace.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader # The "Name" of the access level
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"] # The "Door"
  verbs: ["get", "watch", "list"] # The "Action" (Open, Look through window)
```

### 2.2 RoleBinding (Namespaced)
Grants the permissions defined in a Role to a User or ServiceAccount.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
subjects: # The "Person" or "Robot"
- kind: ServiceAccount
  name: my-app-sa
  namespace: default
roleRef: # The "Access Level" to give them
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

### 2.3 ClusterRole & ClusterRoleBinding
Same as Role/RoleBinding, but for cluster-wide resources (like Nodes) or across all namespaces.
**Analogy**: A **Master Key** that opens every door in the building.

## 3. SecurityContext

Defines privilege and access control settings for a Pod or Container.

**Analogy**: **ID Badge Color**.
- **Root (User 0)**: "Black Badge". Can do anything, go anywhere. Dangerous.
- **Non-Root (User 1000)**: "Green Badge". Can only touch their own stuff. Safer.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo
spec:
  securityContext:
    runAsUser: 1000 # Run as "Green Badge" user
    runAsGroup: 3000
    fsGroup: 2000
  containers:
  - name: sec-ctx-demo
    image: busybox
    command: [ "sh", "-c", "sleep 1h" ]
    securityContext:
      allowPrivilegeEscalation: false # Cannot upgrade to "Black Badge"
```

## Summary
- **ServiceAccounts** (Robot IDs) provide identity to Pods.
- **RBAC** (Key Cards) controls access to the API.
- **Roles** are namespaced; **ClusterRoles** are cluster-wide.
- **SecurityContext** (Badge Color) controls runtime privileges.

[Next: Module 10 - Observability](04-observability.md)
