# Day 67 - Commands

This file contains the commands used during **Day 67 of my 100 Days of DevOps Challenge**.

The objective was to deploy a Guestbook application on Kubernetes using PHP frontend and Redis backend components.

---

## Step 1: Verify Kubernetes Cluster

Check the available Kubernetes nodes:

```bash
kubectl get nodes
```

Check the current namespace:

```bash
kubectl config view --minify --output 'jsonpath={..namespace}'
```

---

## Step 2: Create Project Directory

```bash
mkdir guestbook
cd guestbook
```

Create the YAML manifest:

```bash
nano guestbook.yaml
```

Add the Redis Master, Redis Slave, and Frontend Deployments and Services to this file.

---

## Step 3: Validate YAML Manifest

Perform a client-side dry run:

```bash
kubectl apply --dry-run=client -f guestbook.yaml
```

---

## Step 4: Apply Kubernetes Manifest

Deploy all application components:

```bash
kubectl apply -f guestbook.yaml
```

---

## Step 5: Verify Deployments

Check all Deployments:

```bash
kubectl get deployments
```

Check detailed Deployment information:

```bash
kubectl describe deployment redis-master
kubectl describe deployment redis-slave
kubectl describe deployment frontend
```

---

## Step 6: Verify Rollout Status

Check whether all Deployments have completed their rollout:

```bash
kubectl rollout status deployment/redis-master
kubectl rollout status deployment/redis-slave
kubectl rollout status deployment/frontend
```

---

## Step 7: Verify Pods

List all Pods:

```bash
kubectl get pods
```

Display Pods with node and IP information:

```bash
kubectl get pods -o wide
```

Check Pod labels:

```bash
kubectl get pods --show-labels
```

---

## Step 8: Verify Services

List all Services:

```bash
kubectl get services
```

Short command:

```bash
kubectl get svc
```

Check individual Services:

```bash
kubectl describe service redis-master
kubectl describe service redis-slave
kubectl describe service frontend
```

---

## Step 9: Verify Service Endpoints

Check the endpoints for Redis Master:

```bash
kubectl get endpoints redis-master
```

Check Redis Slave endpoints:

```bash
kubectl get endpoints redis-slave
```

Check frontend endpoints:

```bash
kubectl get endpoints frontend
```

---

## Step 10: Verify Resource Requests

Inspect the Redis Master Deployment:

```bash
kubectl describe deployment redis-master
```

Inspect the Redis Slave Deployment:

```bash
kubectl describe deployment redis-slave
```

Inspect the Frontend Deployment:

```bash
kubectl describe deployment frontend
```

Verify CPU and memory requests in the container specifications.

---

## Step 11: Verify Environment Variables

Check the Redis Slave Pods:

```bash
kubectl describe pods -l component=redis-slave
```

Check the frontend Pods:

```bash
kubectl describe pods -l component=frontend
```

Look for:

```text
GET_HOSTS_FROM=dns
```

---

## Step 12: Verify NodePort

Check the frontend Service:

```bash
kubectl get service frontend
```

Expected port mapping:

```text
80:30009/TCP
```

Inspect the NodePort configuration:

```bash
kubectl describe service frontend
```

---

## Step 13: Check Application Logs

Check Redis Master logs:

```bash
kubectl logs deployment/redis-master
```

Check Redis Slave logs:

```bash
kubectl logs deployment/redis-slave
```

Check frontend logs:

```bash
kubectl logs deployment/frontend
```

If a Deployment has multiple Pods, use `kubectl get pods` and inspect individual Pods when necessary.

---

# Quick Command Summary

```bash
# Check Kubernetes nodes
kubectl get nodes

# Create project directory
mkdir guestbook
cd guestbook

# Create YAML file
nano guestbook.yaml

# Validate manifest
kubectl apply --dry-run=client -f guestbook.yaml

# Deploy Guestbook application
kubectl apply -f guestbook.yaml

# Check Deployments
kubectl get deployments

# Check rollout status
kubectl rollout status deployment/redis-master
kubectl rollout status deployment/redis-slave
kubectl rollout status deployment/frontend

# Check Pods
kubectl get pods -o wide

# Check Services
kubectl get svc

# Check labels
kubectl get pods --show-labels

# Check service endpoints
kubectl get endpoints

# Check frontend NodePort
kubectl get service frontend

# Check application logs
kubectl logs deployment/redis-master
kubectl logs deployment/redis-slave
kubectl logs deployment/frontend
```

---

# Expected Final Configuration

| Resource | Name | Replicas / Type | Port |
|----------|------|-----------------|------|
| Deployment | redis-master | 1 replica | 6379 |
| Service | redis-master | ClusterIP | 6379 |
| Deployment | redis-slave | 2 replicas | 6379 |
| Service | redis-slave | ClusterIP | 6379 |
| Deployment | frontend | 3 replicas | 80 |
| Service | frontend | NodePort | 30009 |

---

# Important Lesson

When deploying a multi-tier application on Kubernetes, it is important to configure each component correctly.

The most important commands used in this challenge were:

```bash
kubectl apply -f guestbook.yaml
kubectl get deployments
kubectl get pods -o wide
kubectl get svc
kubectl describe service frontend
```

These commands helped deploy the application and verify that all Kubernetes resources were configured correctly.

**Day 67 completed successfully.** 🚀
