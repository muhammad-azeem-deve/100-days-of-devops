# Day 65 - Commands

This file contains the commands used during **Day 65 of my 100 Days of DevOps Challenge by KodeKloud**.

The objective was to deploy Redis on Kubernetes using a Deployment, ConfigMap, CPU resource request, and two mounted volumes.

---

# Step 1: Check Kubernetes Cluster

Verify the Kubernetes nodes:

```bash
kubectl get nodes
```

---

# Step 2: Create YAML File

Create the Redis configuration file:

```bash
nano redis-deployment.yaml
```

Add the ConfigMap and Deployment configuration.

---

# Step 3: Apply the Configuration

Deploy the resources to Kubernetes:

```bash
kubectl apply -f redis-deployment.yaml
```

---

# Step 4: Verify ConfigMap

Check the Redis ConfigMap:

```bash
kubectl get configmap my-redis-config
```

View its detailed configuration:

```bash
kubectl describe configmap my-redis-config
```

---

# Step 5: Verify Deployment

Check all Deployments:

```bash
kubectl get deployments
```

Check the Redis Deployment:

```bash
kubectl get deployment redis-deployment
```

View detailed information:

```bash
kubectl describe deployment redis-deployment
```

---

# Step 6: Verify Redis Pod

Check the Pods:

```bash
kubectl get pods
```

Get detailed Pod information:

```bash
kubectl describe pod <redis-pod-name>
```

Check Pods with labels:

```bash
kubectl get pods -l app=redis
```

---

# Step 7: Verify Redis Configuration

Check the configuration file mounted inside the Redis container:

```bash
kubectl exec deployment/redis-deployment -- cat /redis-master/redis-config
```

Expected output:

```text
maxmemory 2mb
```

---

# Step 8: Verify Redis is Working

Execute the Redis CLI inside the container:

```bash
kubectl exec deployment/redis-deployment -- redis-cli ping
```

Expected output:

```text
PONG
```

---

# Step 9: Check Deployment Status

Check the final Deployment status:

```bash
kubectl rollout status deployment/redis-deployment
```

Check the number of available replicas:

```bash
kubectl get deployment redis-deployment
```

Expected result:

```text
READY   UP-TO-DATE   AVAILABLE
1/1     1            1
```

---

# Quick Command Summary

```bash
# Check Kubernetes nodes
kubectl get nodes

# Create YAML file
nano redis-deployment.yaml

# Apply Redis configuration
kubectl apply -f redis-deployment.yaml

# Verify ConfigMap
kubectl get configmap my-redis-config

# Describe ConfigMap
kubectl describe configmap my-redis-config

# Verify Deployment
kubectl get deployments

# Describe Deployment
kubectl describe deployment redis-deployment

# Verify Pods
kubectl get pods

# Verify Redis configuration
kubectl exec deployment/redis-deployment -- cat /redis-master/redis-config

# Test Redis
kubectl exec deployment/redis-deployment -- redis-cli ping

# Verify Deployment rollout
kubectl rollout status deployment/redis-deployment
```

---

# Important Kubernetes Resources

## ConfigMap

```text
Name: my-redis-config

Configuration:
maxmemory 2mb
```

## Deployment

```text
Name: redis-deployment
Image: redis:alpine
Replicas: 1
Container: redis-container
CPU Request: 1
Port: 6379
```

## Volumes

```text
Volume 1:
Name: data
Type: EmptyDir
Mount Path: /redis-master-data

Volume 2:
Name: redis-config
Type: ConfigMap
ConfigMap Name: my-redis-config
Mount Path: /redis-master
```

---

# Important Lesson

In Kubernetes, a Deployment manages Pods, ConfigMaps store application configuration, and Volumes provide storage inside containers.

For this challenge, the most important concepts were:

```text
Deployment
ConfigMap
EmptyDir
VolumeMounts
CPU Requests
Container Ports
```

The main deployment command was:

```bash
kubectl apply -f redis-deployment.yaml
```

And the most important verification commands were:

```bash
kubectl get pods
kubectl get deployments
kubectl get configmap
kubectl exec deployment/redis-deployment -- redis-cli ping
```

**Day 65 completed successfully.** 🚀
