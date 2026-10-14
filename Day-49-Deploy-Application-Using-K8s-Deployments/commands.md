# Day 49 - Commands

This file contains the commands used during Day 49 of my **100 Days of DevOps Challenge by KodeKloud**.

The objective was to deploy an application using a Kubernetes Deployment and verify its status.

The commands below use `nginx-deployment`, the `nginx` image, and three replicas as an example.

---

## Step 1: Check Kubernetes Nodes

```bash
kubectl get nodes
```

## Step 2: Create a Deployment

```bash
kubectl create deployment nginx-deployment --image=nginx --replicas=3
```

## Step 3: Verify Deployments

```bash
kubectl get deployments
```

## Step 4: Verify Pods

```bash
kubectl get pods
```

## Step 5: Check Rollout Status

```bash
kubectl rollout status deployment/nginx-deployment
```

## Step 6: Describe the Deployment

```bash
kubectl describe deployment nginx-deployment
```

## Step 7: Check ReplicaSets

```bash
kubectl get replicasets
```

## Step 8: Check All Resources

```bash
kubectl get all
```

## Step 9: View Deployment YAML

```bash
kubectl get deployment nginx-deployment -o yaml
```

## Step 10: Check Pod Details

```bash
kubectl get pods -o wide
```

---

# Quick Command Summary

```bash
# Check Kubernetes nodes
kubectl get nodes

# Create Deployment with three replicas
kubectl create deployment nginx-deployment --image=nginx --replicas=3

# Check Deployment
kubectl get deployments

# Check Pods
kubectl get pods

# Check rollout
kubectl rollout status deployment/nginx-deployment

# Describe Deployment
kubectl describe deployment nginx-deployment

# Check ReplicaSets
kubectl get replicasets

# Check all resources
kubectl get all

# View Deployment YAML
kubectl get deployment nginx-deployment -o yaml

# Check Pod details
kubectl get pods -o wide
```

---

# Important Lesson

A Kubernetes Deployment manages application Pods and helps maintain the required number of replicas.

The main command used in this example was:

```bash
kubectl create deployment nginx-deployment --image=nginx --replicas=3
```

The most important verification commands were:

```bash
kubectl get deployments
kubectl get pods
kubectl rollout status deployment/nginx-deployment
```

Always use the exact names, container image, and replica count required by the KodeKloud lab.

**Day 49 — Kubernetes Deployments.** 
