# Day 55 - Commands

This file contains the commands used during **Day 55 of my 100 Days of DevOps Challenge**.

The objective was to create a Kubernetes Pod named `webserver` with an Nginx container and an Ubuntu sidecar container that reads Nginx logs through a shared volume.

---

## Step 1: Check the Kubernetes Cluster

```bash
kubectl get nodes
```

This checks whether the Kubernetes nodes are available.

---

## Step 2: Create the YAML File

```bash
vi webserver-pod.yaml
```

Add the required Pod configuration and save the file.

---

## Step 3: Apply the Pod Configuration

```bash
kubectl apply -f webserver-pod.yaml
```

This creates the Pod from the YAML manifest.

---

## Step 4: Verify Pod Status

```bash
kubectl get pods
```

Check the detailed Pod configuration and events:

```bash
kubectl describe pod webserver
```

---

## Step 5: Check the Main Container Logs

```bash
kubectl logs webserver -c nginx-container
```

---

## Step 6: Check the Sidecar Container Logs

```bash
kubectl logs webserver -c sidecar-container
```

---

## Step 7: Inspect the Pod YAML

```bash
kubectl get pod webserver -o yaml
```

This allows verification of the containers, volume mounts, and sidecar configuration.

---

## Step 8: Verify the Containers

```bash
kubectl get pod webserver -o jsonpath='{.status.containerStatuses[*].name}'
```

Check the init-container status:

```bash
kubectl get pod webserver -o jsonpath='{.status.initContainerStatuses[*].name}'
```

The sidecar should appear as an init container with a restart policy of `Always`.

---

# Quick Command Summary

```bash
# Check Kubernetes nodes
kubectl get nodes

# Create the manifest
vi webserver-pod.yaml

# Apply the configuration
kubectl apply -f webserver-pod.yaml

# Check Pod status
kubectl get pods

# Inspect Pod details
kubectl describe pod webserver

# View Nginx logs
kubectl logs webserver -c nginx-container

# View sidecar logs
kubectl logs webserver -c sidecar-container

# Inspect the complete Pod YAML
kubectl get pod webserver -o yaml
```

---

# Important Configuration

The sidecar must use the following image and restart policy:

```yaml
name: sidecar-container
image: ubuntu:latest
restartPolicy: Always
```

The main container must use:

```yaml
name: nginx-container
image: nginx:latest
```

Both containers must mount the shared volume:

```yaml
name: shared-logs
mountPath: /var/log/nginx
```

The volume definition is:

```yaml
volumes:
  - name: shared-logs
    emptyDir: {}
```

---

# Important Lesson

The most important lesson from this challenge is that a sidecar is not simply any second container in a Pod. In this lab, the expected configuration uses `initContainers` with `restartPolicy: Always` for the sidecar.

The shared `emptyDir` volume allows Nginx and the sidecar to access the same log files without requiring a persistent volume.

**Day 55 completed successfully.** 🚀
