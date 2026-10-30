# Day 61 - Commands

This file contains the commands and Kubernetes configuration used during **Day 61 of my 100 Days of DevOps Challenge**.

The objective was to create a Deployment with an Init Container and a Main Container that share data through an `emptyDir` volume.

---

# Step 1: Create YAML File

Create the configuration file:

```bash
nano init-container.yaml
```

---

# Step 2: Kubernetes Deployment Configuration

Add the following configuration:

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: ic-deploy-datacenter
  labels:
    app: ic-datacenter

spec:
  replicas: 1

  selector:
    matchLabels:
      app: ic-datacenter

  template:
    metadata:
      labels:
        app: ic-datacenter

    spec:
      initContainers:
        - name: ic-msg-datacenter
          image: ubuntu:latest
          command:
            - /bin/bash
            - -c
            - "echo 'Welcome to KodeKloud!' > /ic/blog"
          volumeMounts:
            - name: ic-volume-datacenter
              mountPath: /ic

      containers:
        - name: ic-main-datacenter
          image: ubuntu:latest
          command:
            - /bin/bash
            - -c
            - "while true; do cat /ic/blog; sleep 5; done"
          volumeMounts:
            - name: ic-volume-datacenter
              mountPath: /ic

      volumes:
        - name: ic-volume-datacenter
          emptyDir: {}
```

---

# Step 3: Apply the Configuration

Create the Deployment:

```bash
kubectl apply -f init-container.yaml
```

---

# Step 4: Verify Deployment

Check the Deployment:

```bash
kubectl get deployments
```

---

# Step 5: Verify Pods

Check all Pods:

```bash
kubectl get pods
```

Check the Pod using its label:

```bash
kubectl get pods -l app=ic-datacenter
```

---

# Step 6: Get Pod Name

Use the following command to get the Pod name:

```bash
kubectl get pods -l app=ic-datacenter
```

Copy the Pod name and use it in the following commands.

---

# Step 7: Verify Init Container

Check the Init Container logs:

```bash
kubectl logs <pod-name> -c ic-msg-datacenter
```

The Init Container may produce no output because it writes the welcome message into a file.

---

# Step 8: Verify Main Container

Check the Main Container logs:

```bash
kubectl logs <pod-name> -c ic-main-datacenter
```

Expected output:

```text
Welcome to KodeKloud!
Welcome to KodeKloud!
Welcome to KodeKloud!
```

The message is displayed repeatedly every 5 seconds.

---

# Step 9: Verify Shared File

Check the file inside the Main Container:

```bash
kubectl exec <pod-name> -c ic-main-datacenter -- cat /ic/blog
```

Expected output:

```text
Welcome to KodeKloud!
```

---

# Step 10: Inspect Pod Details

Check detailed Pod information:

```bash
kubectl describe pod <pod-name>
```

This command helps verify:

- Init Container details.
- Main Container details.
- Volume configuration.
- Volume mounts.
- Container status.
- Pod events.

---

# Quick Command Summary

```bash
# Create YAML file
nano init-container.yaml

# Apply Deployment
kubectl apply -f init-container.yaml

# Verify Deployment
kubectl get deployments

# Check Pods
kubectl get pods

# Find Pod using label
kubectl get pods -l app=ic-datacenter

# Check Init Container logs
kubectl logs <pod-name> -c ic-msg-datacenter

# Check Main Container logs
kubectl logs <pod-name> -c ic-main-datacenter

# Verify shared file
kubectl exec <pod-name> -c ic-main-datacenter -- cat /ic/blog

# Inspect Pod details
kubectl describe pod <pod-name>
```

---

# Understanding Important Kubernetes Concepts

## 1. Init Container

```yaml
initContainers:
  - name: ic-msg-datacenter
```

An Init Container runs before the Main Container and performs initialization tasks.

## 2. Main Container

```yaml
containers:
  - name: ic-main-datacenter
```

The Main Container starts after the Init Container finishes successfully.

## 3. emptyDir Volume

```yaml
volumes:
  - name: ic-volume-datacenter
    emptyDir: {}
```

Creates temporary shared storage for containers in the same Pod.

## 4. Volume Mount

```yaml
volumeMounts:
  - name: ic-volume-datacenter
    mountPath: /ic
```

Mounts the shared volume inside the container at `/ic`.

## 5. Infinite Loop

```bash
while true; do cat /ic/blog; sleep 5; done
```

Continuously reads the file and waits five seconds between executions.

---

# Important Lesson

In Kubernetes, Init Containers are useful when an application needs some preparation before starting.

For this challenge:

- The Init Container created the welcome message file.
- The `emptyDir` volume shared the file between containers.
- The Main Container read and displayed the file every 5 seconds.

The most important concepts were:

```text
Init Container
emptyDir Volume
volumeMounts
Main Container
```

**Day 61 completed successfully.** 🚀
