# Module 06: Storage Basics

## Learning Objectives
By the end of this module, you will be able to:
- Understand how storage works in Kubernetes (The Parking Lot).
- Create PersistentVolumes (PV) and PersistentVolumeClaims (PVC).
- Use StorageClasses for dynamic provisioning (The Valet).

## 1. The Storage Problem

Containers are ephemeral. When a container crashes, the data inside it is lost.
**Problem**: If your database container restarts, you lose all your user data.
**Solution**: **Volumes**. They decouple storage from the container lifecycle.

## 2. PV and PVC

Kubernetes separates the "Physical Storage" from the "Request for Storage".

**Analogy**: **Parking Lot**.
- **PV (PersistentVolume)**: A specific parking spot (Spot #101). It exists whether a car is parked there or not.
- **PVC (PersistentVolumeClaim)**: A parking ticket/reservation. "I need a spot for a large SUV."

### 2.1 PersistentVolume (PV)
A piece of storage in the cluster that has been provisioned by an administrator.
**Analogy**: The building manager painting lines for parking spots.

### 2.2 PersistentVolumeClaim (PVC)
A request for storage by a user.
**Analogy**: You (the driver) asking for a spot. You don't care *which* spot (Spot #55 or #56), you just want *a* spot that fits your car.

### 2.3 How they work together
1. **Admin** creates a PV (Paints a spot).
2. **User** creates a PVC (Asks for a spot).
3. **Kubernetes** binds the PVC to a matching PV (Assigns you Spot #101).
4. **Pod** uses the PVC as a volume (You park your car).

## 3. StorageClass (Dynamic Provisioning)

Manually creating PVs is tedious.
**Analogy**: **Valet Parking**.
- You don't look for a spot.
- You just hand your keys (PVC) to the Valet (StorageClass).
- The Valet *creates* a spot for you (provisions a disk from AWS/GCP) and parks your car.

### 3.1 Example PVC using Default StorageClass
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce # Only one node can mount this at a time
  resources:
    requests:
      storage: 1Gi # I need 1GB of space
  # storageClassName: standard (omitted to use default)
```

### 3.2 Using PVC in a Pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: storage-pod
spec:
  containers:
    - name: task-pv-container
      image: nginx
      ports:
        - containerPort: 80
          name: "http-server"
      volumeMounts:
        - mountPath: "/usr/share/nginx/html" # Where the data appears inside the container
          name: task-pv-storage
  volumes:
    - name: task-pv-storage
      persistentVolumeClaim:
        claimName: my-pvc # Link to the Ticket
```

## Summary
- **Volumes** persist data beyond container life.
- **PV** (Parking Spot) is the physical storage.
- **PVC** (Parking Ticket) is the request for storage.
- **StorageClass** (Valet) automates PV creation.

[Next: Intermediate Module 01 - Advanced Workloads](../intermediate/01-advanced-workloads.md)
