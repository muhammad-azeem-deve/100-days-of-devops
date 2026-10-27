# Day 60 - Persistent Volumes in Kubernetes

## Challenge Overview

This is Day 60 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Kubernetes Persistent Volumes and Persistent Volume Claims**. The task required creating a 4 Gi Persistent Volume, requesting 3 Gi of storage through a PVC, mounting the requested storage inside an Apache web server Pod, and exposing the application through a NodePort Service.

### Challenge Requirement

The task required the following resources:

- Create a Persistent Volume with `4Gi` capacity.
- Configure the storage class as `manual`.
- Use `/mnt/sysops` as the local storage path.
- Create a Persistent Volume Claim requesting `3Gi`.
- Run an Apache container using `httpd:latest`.
- Mount the PVC at Apache's default web directory.
- Expose the web server using NodePort `30008`.

---

## Objectives

The objectives of this challenge are:

- Understand Kubernetes persistent storage.
- Create a Persistent Volume (PV).
- Create a Persistent Volume Claim (PVC).
- Understand PV and PVC binding.
- Deploy an Apache web server using a Pod.
- Mount persistent storage inside a container.
- Expose the web server through a NodePort Service.
- Verify Kubernetes resources using `kubectl`.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 60 |
| Storage Type | `hostPath` |
| PV Name | `pv-sysops` |
| PV Capacity | `4Gi` |
| Storage Class | `manual` |
| Host Path | `/mnt/sysops` |
| PVC Name | `pvc-sysops` |
| PVC Request | `3Gi` |
| Pod Name | `pod-sysops` |
| Container Name | `container-sysops` |
| Image | `httpd:latest` |
| Service Name | `web-sysops` |
| Service Type | `NodePort` |
| NodePort | `30008` |
| Status | Completed |

---

# Solution

## Step 1: Check the Kubernetes Cluster

First, I checked the available Kubernetes nodes and existing resources.

```bash
kubectl get nodes
kubectl get pv
kubectl get pvc
kubectl get pods
```

These commands helped me understand the current cluster state before creating the required resources.

## Step 2: Create the Persistent Volume

I created a file named `pv.yaml`.

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

I applied the configuration:

```bash
kubectl apply -f pv.yaml
```

I verified the Persistent Volume:

```bash
kubectl get pv
kubectl describe pv pv-sysops
```

The PV provides `4Gi` of storage using the local host path `/mnt/sysops`.

## Step 3: Create the Persistent Volume Claim

Next, I created `pvc.yaml` to request `3Gi` of storage.

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

I applied the configuration:

```bash
kubectl apply -f pvc.yaml
```

I verified the claim:

```bash
kubectl get pvc
kubectl describe pvc pvc-sysops
```

The PVC requests `3Gi` of storage from a compatible Persistent Volume.

## Step 4: Create the Apache Pod

I created `pod.yaml` to run Apache and mount the requested storage.

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

I applied the configuration:

```bash
kubectl apply -f pod.yaml
```

I checked the Pod status:

```bash
kubectl get pods
kubectl describe pod pod-sysops
```

The Pod uses the `pvc-sysops` claim to access persistent storage.

## Step 5: Create the NodePort Service

I created `service.yaml` to expose the Apache web server.

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

I applied the configuration:

```bash
kubectl apply -f service.yaml
```

I verified the Service:

```bash
kubectl get svc web-sysops
kubectl describe svc web-sysops
```

The Service exposes the application through NodePort `30008`.

## Step 6: Verify Persistent Storage

I checked the PV and PVC status:

```bash
kubectl get pv
kubectl get pvc
```

The PVC should show `Bound`, indicating that Kubernetes successfully associated the claim with a compatible PV.

## Step 7: Verify the Apache Pod

I checked whether the Pod was running:

```bash
kubectl get pods -o wide
```

I checked its logs:

```bash
kubectl logs pod-sysops
```

I also tested Apache from inside the Pod:

```bash
kubectl exec pod-sysops -- curl -I http://localhost
```

A successful HTTP response indicates that Apache is responding to requests.

## Step 8: Verify the Service

I checked the Service and its endpoints:

```bash
kubectl get svc web-sysops
kubectl get endpoints web-sysops
```

The Service should have a matching endpoint for the Apache Pod.

The application can then be tested through the lab's supported browser access method using NodePort `30008`.

---

# Understanding Kubernetes Storage

### Persistent Volume (PV)

A PV represents storage that Kubernetes can allocate to workloads.

In this challenge, the PV has:

- Capacity: `4Gi`
- Storage class: `manual`
- Access mode: `ReadWriteOnce`
- Host path: `/mnt/sysops`

### Persistent Volume Claim (PVC)

A PVC requests storage from Kubernetes.

In this challenge, the PVC requests `3Gi` and uses the `manual` storage class to match the PV.

### Pod

The Pod runs the Apache container and mounts the PVC at:

```text
/usr/local/apache2/htdocs/
```

This is Apache's default document root.

### NodePort Service

The Service exposes Apache through port `30008` on the Kubernetes nodes.

---

# What I Learned

From this challenge, I learned and practiced:

- How Persistent Volumes provide storage in Kubernetes.
- How Persistent Volume Claims request storage.
- How Kubernetes binds a PVC to a compatible PV.
- How `hostPath` storage works.
- How to mount a PVC inside a container.
- How to deploy Apache using `httpd:latest`.
- How Kubernetes Pods use volumes.
- How to expose an application through a NodePort Service.
- How to configure Service selectors and Pod labels.
- How to verify storage and networking resources using `kubectl`.

# Important Lesson

A Persistent Volume provides storage, a Persistent Volume Claim requests that storage, and a Pod consumes it through a volume mount.

A NodePort Service provides network access to the application, but it does not create or manage persistent storage.

**Note:** `hostPath` storage is tied to a particular Kubernetes node. It is useful for labs and testing, but it is not a substitute for properly managed shared or durable storage in production environments.

# Challenge Status

Day 60 — Completed Successfully ✅

**Persistent Storage + Apache + NodePort = Another Kubernetes Challenge Completed!**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
