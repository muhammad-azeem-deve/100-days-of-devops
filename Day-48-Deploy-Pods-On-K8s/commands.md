# Day 48 - Commands

This file contains the commands used during **Day 48 of my 100 Days of DevOps Challenge**.

The objective is to deploy a Kubernetes Pod and verify its configuration.

> The commands below use `pod-nginx` and `nginx:latest` as example values. Use the exact Pod name and image required by your lab.

---

## Step 1: Access the Kubernetes Server

```bash
ssh <username>@<server-hostname>
```

## Step 2: Verify Kubernetes Nodes

```bash
kubectl get nodes
```

## Step 3: Create the Pod

```bash
kubectl run pod-nginx --image=nginx:latest
```

## Step 4: Check All Pods

```bash
kubectl get pods
```

## Step 5: Check the Pod in Detail

```bash
kubectl describe pod pod-nginx
```

## Step 6: Verify the Pod Image

```bash
kubectl get pod pod-nginx -o jsonpath='{.spec.containers[*].image}'
```

Expected output:

```text
nginx:latest
```

## Step 7: View Pod Logs

```bash
kubectl logs pod-nginx
```

## Step 8: Check Pod Events

```bash
kubectl get events --sort-by=.metadata.creationTimestamp
```

## Step 9: Verify the Pod Status

```bash
kubectl get pod pod-nginx -o wide
```

The output includes the Pod status, IP address, and node.

---

# Quick Command Summary

```bash
# Check Kubernetes nodes
kubectl get nodes

# Create a Pod
kubectl run pod-nginx --image=nginx:latest

# List Pods
kubectl get pods

# Inspect the Pod
kubectl describe pod pod-nginx

# Verify the configured image
kubectl get pod pod-nginx -o jsonpath='{.spec.containers[*].image}'

# View container logs
kubectl logs pod-nginx

# Check cluster events
kubectl get events --sort-by=.metadata.creationTimestamp

# Check Pod details
kubectl get pod pod-nginx -o wide
```

---

# Important Lesson

The most important command in this practice is:

```bash
kubectl run pod-nginx --image=nginx:latest
```

It creates a Pod using the specified image.

Always verify the result using:

```bash
kubectl get pods
```

A successful command does not necessarily mean the application has started successfully. Check the Pod status and investigate events if the Pod does not reach `Running`.

**Day 48 — Kubernetes Pod Deployment Practice.** 
