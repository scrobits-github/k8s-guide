# Module 05: Configuration

## Learning Objectives
By the end of this module, you will be able to:
- Decouple configuration from your application code (The 12-Factor App way).
- Use ConfigMaps for non-sensitive data (Sticky Notes).
- Use Secrets for sensitive data (The Safe).
- Inject configuration as Environment Variables or Files.

## 1. The 12-Factor App Philosophy

Modern cloud-native applications follow the **12-Factor App** methodology.
**Factor III: Config** states: *"Store config in the environment"*.

### Why?
- **Traditional Way**: Hardcoding config in `config.js` or `settings.py`.
  - **Problem**: You need to rebuild the code to change the database URL from Dev to Prod.
- **The K8s Way**: The code is the same everywhere (Docker Image). The *config* is injected from the outside.
  - **Benefit**: Build once, run anywhere.

## 2. ConfigMaps

A **ConfigMap** is an API object used to store non-confidential data in key-value pairs.

**Analogy**: **Sticky Notes** or a **Whiteboard**.
- You write "Theme = Dark Mode" on a sticky note.
- Anyone in the room (Pod) can read it.
- If you want to change the theme, you just write a new note. You don't have to rebuild the entire room.

### 2.1 Creating a ConfigMap
Create `configmap.yaml`:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: game-config
data:
  player_lives: "3"
  ui_properties_file_name: "user-interface.properties"
```

### 2.2 Using a ConfigMap (Env Vars)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: config-pod
spec:
  containers:
    - name: test-container
      image: k8s.gcr.io/busybox
      command: [ "/bin/sh", "-c", "env" ]
      env:
        # Define the environment variable
        - name: PLAYER_LIVES
          valueFrom:
            configMapKeyRef:
              name: game-config
              key: player_lives
```

## 3. Secrets

A **Secret** is similar to a ConfigMap but is intended to hold small amounts of sensitive data such as passwords, OAuth tokens, and SSH keys.

**Analogy**: A **Locked Safe**.
- You put the "Nuclear Launch Codes" inside.
- Only people with the right key (ServiceAccount permissions) can open it.
- To the casual observer, it just looks like a metal box (Base64 encoded string).

> [!IMPORTANT]
> Secrets are base64 encoded, NOT encrypted by default. It's like writing a password in a secret language that is easy to translate. Enable encryption at rest in etcd for production.

### 3.1 Creating a Secret
```bash
# Imperative
kubectl create secret generic my-secret --from-literal=password=123456
```

### 3.2 Using a Secret (Volume Mount)
Sometimes apps expect config files, not env vars. You can mount Secrets (and ConfigMaps) as files.

**Analogy**: Putting the secret document **inside a folder** on the worker's desk.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-pod
spec:
  containers:
  - name: my-container
    image: redis
    volumeMounts:
    - name: secret-volume
      mountPath: "/etc/secret-volume" # The folder on the desk
      readOnly: true
  volumes:
  - name: secret-volume
    secret:
      secretName: my-secret
```

## Summary
- **ConfigMaps** (Sticky Notes) store configuration.
- **Secrets** (The Safe) store sensitive data.
- Both can be injected as **Environment Variables** or mounted as **Files** (Volumes).
- **Decoupling** config from code makes your apps portable (Write once, run anywhere).

[Next: Module 06 - Storage Basics](06-storage-basics.md)
