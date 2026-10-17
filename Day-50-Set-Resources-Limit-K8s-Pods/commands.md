# Day 50 - Commands

This file contains the commands used during **Day 50 of my 100 Days of DevOps Challenge by KodeKloud**.

The objective was to create a Kubernetes Pod using a YAML file and configure CPU and memory resource requests and limits.

---

## Step 1: Create the YAML File

```bash
nano pod.yaml
```

Add the Pod configuration and save the file.

---

## Step 2: Validate the YAML File

```bash
cat pod.yaml
```

Review the file to ensure the indentation and resource values are correct.

---

## Step 3: Create the Pod

```bash
kubectl apply -f pod.yaml
```

This command creates or updates the Kubernetes resource defined in the YAML file.

---

## Step 4: Verify the Pod

```bash
kubectl get pods
```

Check whether the Pod appears in the output.

---

## Step 5: Check Pod Details

```bash
kubectl describe pod resource-pod
```

Inspect the Pod details, including the container image, events, resource requests, and resource limits.

---

## Step 6: Verify the Saved YAML

```bash
kubectl get pod resource-pod -o yaml
```

This displays the configuration stored in Kubernetes.

---

## Step 7: Check Pod Status in Detail

```bash
kubectl get pod resource-pod -o wide
```

This displays the Pod status along with additional information, such as the node on which it is scheduled.

---

# Example Resource Configuration

The following is an example of the resource section in a Pod YAML file:

```yaml
resources:
  requests:
    cpu: "100m"
    memory: "64Mi"
  limits:
    cpu: "200m"
    memory: "128Mi"
```

Use the exact values specified in the KodeKloud task.

---

# Quick Command Summary

```bash
# Create the YAML file
nano pod.yaml

# Review the YAML file
cat pod.yaml

# Create or update the Pod
kubectl apply -f pod.yaml

# List Pods
kubectl get pods

# Inspect the Pod and resource settings
kubectl describe pod resource-pod

# View the saved Pod configuration
kubectl get pod resource-pod -o yaml

# Display additional Pod details
kubectl get pod resource-pod -o wide
```

---

# Important Lesson

A Kubernetes Pod can be defined using a YAML file, allowing its container image and resource requirements to be managed declaratively.

The key resource settings are:

```yaml
resources:
  requests:
    cpu: "100m"
    memory: "64Mi"
  limits:
    cpu: "200m"
    memory: "128Mi"
```

Resource requests help Kubernetes schedule the Pod, while resource limits control the container's maximum CPU and memory consumption.

Always use the exact Pod name, image, and resource values required by the lab, then verify the configuration using `kubectl describe`.

**Day 50 completed successfully.** 
