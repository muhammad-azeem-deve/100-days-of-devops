# Day 60 - Persistent Volumes in Kubernetes

This file contains the commands used during **Day 60 of my 100 Days of DevOps Challenge by KodeKloud**.

The objective was to create a 4 Gi Persistent Volume, request 3 Gi through a PVC, mount the storage in an Apache Pod, and expose the web server using NodePort `30008`.

---

## Step 1: Check the Cluster

```bash
kubectl get nodes
kubectl get pv
kubectl get pvc
kubectl get pods
```

---

## Step 2: Create the Persistent Volume

Create the manifest file:

```bash
vi pv.yaml
```

Add the following YAML:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-sysops
spec:
  capacity:
    storage: 4Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: manual
  hostPath:
    path: /mnt/sysops
```

Apply the manifest:

```bash
kubectl apply -f pv.yaml
```

Verify the PV:

```bash
kubectl get pv
kubectl describe pv pv-sysops
```

---

## Step 3: Create the Persistent Volume Claim

Create the manifest file:

```bash
vi pvc.yaml
```

Add:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-sysops
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: manual
  resources:
    requests:
      storage: 3Gi
```

Apply the manifest:

```bash
kubectl apply -f pvc.yaml
```

Verify the PVC:

```bash
kubectl get pvc
kubectl describe pvc pvc-sysops
```

---

## Step 4: Create the Apache Pod

Create the manifest file:

```bash
vi pod.yaml
```

Add:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-sysops
  labels:
    app: web-sysops
spec:
  containers:
    - name: container-sysops
      image: httpd:latest
      ports:
        - containerPort: 80
      volumeMounts:
        - name: storage
          mountPath: /usr/local/apache2/htdocs/
  volumes:
    - name: storage
      persistentVolumeClaim:
        claimName: pvc-sysops
```

Apply the manifest:

```bash
kubectl apply -f pod.yaml
```

Verify the Pod:

```bash
kubectl get pods
kubectl describe pod pod-sysops
```

---

## Step 5: Create the NodePort Service

Create the manifest file:

```bash
vi service.yaml
```

Add:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-sysops
spec:
  type: NodePort
  selector:
    app: web-sysops
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30008
```

Apply the manifest:

```bash
kubectl apply -f service.yaml
```

Verify the Service:

```bash
kubectl get svc web-sysops
kubectl describe svc web-sysops
```

---

## Step 6: Verify PV and PVC Binding

```bash
kubectl get pv
kubectl get pvc
```

The expected PVC status is:

```text
Bound
```

---

## Step 7: Verify the Pod

```bash
kubectl get pods -o wide
kubectl describe pod pod-sysops
kubectl logs pod-sysops
```

Test Apache from inside the Pod:

```bash
kubectl exec pod-sysops -- curl -I http://localhost
```

---

## Step 8: Verify the Service Endpoints

```bash
kubectl get svc web-sysops
kubectl get endpoints web-sysops
```

The Service should have an endpoint corresponding to the Apache Pod.

---

# Quick Command Summary

```bash
# Check the cluster
kubectl get nodes
kubectl get pv
kubectl get pvc
kubectl get pods

# Create and apply Persistent Volume
vi pv.yaml
kubectl apply -f pv.yaml

# Create and apply Persistent Volume Claim
vi pvc.yaml
kubectl apply -f pvc.yaml

# Create and apply Apache Pod
vi pod.yaml
kubectl apply -f pod.yaml

# Create and apply NodePort Service
vi service.yaml
kubectl apply -f service.yaml

# Verify storage
kubectl get pv
kubectl get pvc

# Verify Pod
kubectl get pods -o wide
kubectl logs pod-sysops

# Test Apache
kubectl exec pod-sysops -- curl -I http://localhost

# Verify Service
kubectl get svc web-sysops
kubectl get endpoints web-sysops
```

---

# Important Kubernetes Concepts

| Component | Purpose |
|-----------|---------|
| PV | Provides persistent storage |
| PVC | Requests storage |
| Pod | Runs the Apache container |
| Volume Mount | Connects storage to the container |
| Service | Exposes the application |
| NodePort | Exposes the Service on a node port |

---

# Important Lesson

The most important part of this task is connecting the resources correctly:

1. The PVC must match a suitable PV.
2. The Pod must reference the correct PVC.
3. The Service selector must match the Pod labels.
4. The Service must use NodePort `30008`.
5. The Pod and Service must be healthy before testing external access.

**Day 60 completed successfully.** 🚀
