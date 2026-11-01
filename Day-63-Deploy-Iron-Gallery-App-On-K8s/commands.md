# Day 63 - Commands

This file contains the commands used during **Day 63 of my 100 Days of DevOps Challenge by KodeKloud**.

The objective was to deploy the Iron Gallery frontend and MariaDB database on Kubernetes using Deployments, Services, Namespace, and `emptyDir` volumes.

---

# Step 1: Verify Kubernetes Cluster

Check the available Kubernetes nodes:

```bash
kubectl get nodes
```

Check namespaces:

```bash
kubectl get namespaces
```

---

# Step 2: Create the YAML Manifest

Create the YAML file:

```bash
nano iron-gallery.yaml
```

Add the Namespace, both Deployments, and both Services to the file.

Use `---` to separate multiple Kubernetes resources.

---

# Step 3: Validate the YAML Manifest

Check the YAML syntax and Kubernetes resource validity:

```bash
kubectl apply --dry-run=client -f iron-gallery.yaml
```

---

# Step 4: Apply the Manifest

Deploy all Kubernetes resources:

```bash
kubectl apply -f iron-gallery.yaml
```

---

# Step 5: Verify Namespace

```bash
kubectl get namespaces
```

Check the specific namespace:

```bash
kubectl get namespace iron-namespace-xfusion
```

---

# Step 6: Verify Deployments

```bash
kubectl get deployments -n iron-namespace-xfusion
```

Get detailed Deployment information:

```bash
kubectl describe deployment iron-gallery-deployment-xfusion -n iron-namespace-xfusion
```

```bash
kubectl describe deployment iron-db-deployment-xfusion -n iron-namespace-xfusion
```

---

# Step 7: Verify Pods

List all Pods:

```bash
kubectl get pods -n iron-namespace-xfusion
```

Watch Pod status:

```bash
kubectl get pods -n iron-namespace-xfusion -w
```

Get detailed Pod information:

```bash
kubectl describe pod <pod-name> -n iron-namespace-xfusion
```

---

# Step 8: Check Container Logs

Frontend logs:

```bash
kubectl logs deployment/iron-gallery-deployment-xfusion -n iron-namespace-xfusion
```

Database logs:

```bash
kubectl logs deployment/iron-db-deployment-xfusion -n iron-namespace-xfusion
```

---

# Step 9: Verify Services

```bash
kubectl get services -n iron-namespace-xfusion
```

Get detailed frontend Service information:

```bash
kubectl describe service iron-gallery-service-xfusion -n iron-namespace-xfusion
```

Get detailed database Service information:

```bash
kubectl describe service iron-db-service-xfusion -n iron-namespace-xfusion
```

---

# Step 10: Verify Service Endpoints

```bash
kubectl get endpoints -n iron-namespace-xfusion
```

---

# Step 11: Verify All Resources

Display all resources inside the namespace:

```bash
kubectl get all -n iron-namespace-xfusion
```

Display resources with more details:

```bash
kubectl get all -n iron-namespace-xfusion -o wide
```

---

# Step 12: Access the Frontend

Get the Kubernetes node IP:

```bash
kubectl get nodes -o wide
```

Access the application using:

```text
http://<KUBERNETES-NODE-IP>:32678
```

The frontend should display the Iron Gallery installation page.

---

# Quick Command Summary

```bash
# Check Kubernetes nodes
kubectl get nodes

# Check namespaces
kubectl get namespaces

# Create YAML manifest
nano iron-gallery.yaml

# Validate YAML manifest
kubectl apply --dry-run=client -f iron-gallery.yaml

# Deploy all resources
kubectl apply -f iron-gallery.yaml

# Verify namespace
kubectl get namespace iron-namespace-xfusion

# Verify deployments
kubectl get deployments -n iron-namespace-xfusion

# Verify pods
kubectl get pods -n iron-namespace-xfusion

# Watch pods
kubectl get pods -n iron-namespace-xfusion -w

# Verify services
kubectl get services -n iron-namespace-xfusion

# Verify endpoints
kubectl get endpoints -n iron-namespace-xfusion

# Verify all resources
kubectl get all -n iron-namespace-xfusion

# Get node IP
kubectl get nodes -o wide
```

---

# Troubleshooting Commands

## 1. Check Pod Status

```bash
kubectl get pods -n iron-namespace-xfusion
```

## 2. Describe a Pod

```bash
kubectl describe pod <pod-name> -n iron-namespace-xfusion
```

## 3. Check Container Logs

```bash
kubectl logs <pod-name> -n iron-namespace-xfusion
```

## 4. Check Deployment Status

```bash
kubectl rollout status deployment/iron-gallery-deployment-xfusion -n iron-namespace-xfusion
```

```bash
kubectl rollout status deployment/iron-db-deployment-xfusion -n iron-namespace-xfusion
```

## 5. Check Service Endpoints

```bash
kubectl get endpoints -n iron-namespace-xfusion
```

## 6. Check YAML Configuration

```bash
kubectl get deployment iron-gallery-deployment-xfusion -n iron-namespace-xfusion -o yaml
```

```bash
kubectl get deployment iron-db-deployment-xfusion -n iron-namespace-xfusion -o yaml
```

---

# Important Kubernetes Concepts

```text
Namespace  → Logical isolation of resources.

Deployment → Manages and maintains Pods.

Pod        → Runs application containers.

Service    → Provides stable network access to Pods.

NodePort   → Exposes the frontend outside the cluster.

ClusterIP  → Provides internal database access.

emptyDir   → Provides temporary Pod storage.

kubectl    → Command-line tool for managing Kubernetes.
```

---

# Important Lesson

The most important command in this challenge was:

```bash
kubectl apply -f iron-gallery.yaml
```

Using one declarative YAML manifest made it easier to manage all five Kubernetes resources together.

Always verify:

1. Namespace creation.
2. Deployment availability.
3. Pod status.
4. Service configuration.
5. Service endpoints.
6. Frontend accessibility.

**Day 63 completed successfully.** 🚀
