# Module 11: Helm & Packaging

## Learning Objectives
By the end of this module, you will be able to:
- Understand what Helm is and why it's useful (The App Store).
- Install and manage applications using Helm Charts (The Recipe).
- Create a basic Helm Chart.

## 1. What is Helm?

Helm is the **package manager** for Kubernetes.

**Analogy**: **The App Store** (or `apt`/`yum` for Linux).
- **Kubernetes YAMLs**: Manually downloading a `.zip` file, extracting it, moving files to the right folders, and configuring paths.
- **Helm**: Clicking "Install" on the App Store.

It helps you manage Kubernetes applications — Helm Charts help you define, install, and upgrade even the most complex Kubernetes application.

### Key Concepts
- **Chart**: A package of pre-configured Kubernetes resources.
  - **Analogy**: The **Recipe** (e.g., "Chocolate Cake Recipe").
- **Release**: A specific instance of a chart running in a cluster.
  - **Analogy**: The **Cake** itself. You can bake 5 cakes (Releases) from 1 recipe (Chart).
- **Repository**: A place where charts can be collected and shared.
  - **Analogy**: The **Cookbook** or **App Store Server**.

## 2. Using Helm

### 2.1 Install Helm
**MacOS (Homebrew)**:
```bash
brew install helm
```

### 2.2 Add a Repository
Before you can install apps, you need to add the "Store" to your list.

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

### 2.3 Install a Chart
```bash
# Install MySQL
# "Bake me a MySQL cake and call it 'my-db'"
helm install my-db bitnami/mysql
```

### 2.4 List Releases
```bash
# "Show me all the cakes I've baked"
helm list
```

### 2.5 Uninstall a Release
```bash
# "Throw this cake in the trash"
helm uninstall my-db
```

## 3. Creating a Chart

### 3.1 Create the structure
```bash
helm create my-chart
```
This creates a directory `my-chart` with standard files (`Chart.yaml`, `values.yaml`, `templates/`).

### 3.2 The `values.yaml` file
This is where you define default configuration values. Users can override these when installing the chart.

**Analogy**: **Customizing the Order**.
- "I want the cake, BUT with extra frosting and no nuts."
- `values.yaml` lists the defaults (Standard Frosting, With Nuts).

```yaml
# values.yaml
replicaCount: 1
image:
  repository: nginx
  tag: stable
```

### 3.3 Templates
Files in `templates/` use the Go templating language. They take the `values.yaml` and inject them into the YAML files.

```yaml
# templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
spec:
  replicas: {{ .Values.replicaCount }} # Injects "1" from above
  ...
```

### 3.4 Install your local chart
```bash
helm install my-local-app ./my-chart
```

## Summary
- **Helm** (App Store) simplifies app deployment.
- **Charts** (Recipes) are reusable packages.
- **Releases** (Cakes) are running instances.
- **Values** (Customization) allow you to change the recipe without rewriting it.

[Next: Advanced Modules](../advanced/01-cluster-architecture-deep-dive.md)
