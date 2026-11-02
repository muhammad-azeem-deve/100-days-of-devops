# Day 64 - Commands

This file contains the commands used during **Day 64 of my 100 Days of DevOps Challenge**.

The objective was to fix a Python application deployed on Kubernetes by correcting the container image and Service target port.

---

# Step 1: Check Kubernetes Cluster

Check the available Kubernetes nodes:

```bash
kubectl get nodes
```

---

# Step 2: Check Existing Resources

Check Pods:

```bash
kubectl get pods
```

Check Deployments:

```bash
kubectl get deployments
```

Check Services:

```bash
kubectl get services
```

Check all resources together:

```bash
kubectl get all
```

---

# Step 3: Check Deployment Status

```bash
kubectl get deployment python-deployment-devops
```

---

# Step 4: Inspect the Application Pod

List the Pods:

```bash
kubectl get pods
```

Describe the application Pod:

```bash
kubectl describe pod <pod-name>
```

Check the Pod logs:

```bash
kubectl logs <pod-name>
```

Look for errors such as:

```text
ImagePullBackOff
ErrImagePull
```

---

# Step 5: Fix the Container Image

Edit the Deployment:

```bash
kubectl edit deployment python-deployment-devops
```

Find the container image:

```yaml
containers:
  - name: python-container-devops
    image: wrong-image
```

Replace it with the correct image specified in the challenge.

Example for the commonly documented version:

```yaml
image: poroko/flask-demo-app
```

Save and exit the editor.

---

# Step 6: Verify Deployment Rollout

Check the Deployment rollout:

```bash
kubectl rollout status deployment/python-deployment-devops
```

Check Deployment status:

```bash
kubectl get deployment
```

Check Pod status:

```bash
kubectl get pods
```

---

# Step 7: Check Existing Service

List the Services:

```bash
kubectl get svc
```

Describe the Python application Service:

```bash
kubectl describe svc python-service-devops
```

Check the existing port configuration.

---

# Step 8: Fix the Service Target Port

Edit the Service:

```bash
kubectl edit svc python-service-devops
```

Find:

```yaml
ports:
  - port: 8080
    targetPort: 8080
    nodePort: 32345
```

Change the target port:

```yaml
ports:
  - port: 8080
    targetPort: 5000
    nodePort: 32345
```

Save and exit the editor.

---

# Step 9: Verify Service Configuration

Check the Service:

```bash
kubectl get svc python-service-devops
```

Describe the Service:

```bash
kubectl describe svc python-service-devops
```

Expected configuration:

```text
Port:        8080/TCP
TargetPort:  5000/TCP
NodePort:    32345/TCP
```

---

# Step 10: Verify Service Endpoints

```bash
kubectl get endpoints python-service-devops
```

The endpoint should point to the application Pod on port `5000`.

---

# Step 11: Test Application Accessibility

Test the application using curl:

```bash
curl http://<node-ip>:32345
```

Replace `<node-ip>` with the reachable IP address of a Kubernetes node.

---

# Quick Command Summary

```bash
# Check Kubernetes nodes
kubectl get nodes

# Check existing resources
kubectl get pods
kubectl get deployments
kubectl get services

# Check Deployment
kubectl get deployment python-deployment-devops

# Inspect Pod
kubectl describe pod <pod-name>

# Check Pod logs
kubectl logs <pod-name>

# Edit Deployment and correct image
kubectl edit deployment python-deployment-devops

# Verify Deployment rollout
kubectl rollout status deployment/python-deployment-devops

# Verify Pod status
kubectl get pods

# Inspect Service
kubectl describe svc python-service-devops

# Edit Service and correct targetPort
kubectl edit svc python-service-devops

# Verify Service
kubectl get svc python-service-devops

# Verify Service endpoints
kubectl get endpoints python-service-devops

# Test application
curl http://<node-ip>:32345
```

---

# Important Kubernetes Configurations

## Correct Container Image

Use the exact image specified in the challenge.

Example:

```yaml
image: poroko/flask-demo-app
```

## Correct Service Configuration

```yaml
ports:
  - port: 8080
    targetPort: 5000
    nodePort: 32345
```

---

# Understanding Kubernetes Ports

```text
NodePort:   32345
Service:    8080
TargetPort: 5000
```

- `NodePort`: Provides external access to the application through a Kubernetes node.
- `port`: Exposes the application through the Kubernetes Service.
- `targetPort`: Forwards traffic to the application container.

---

# Important Lesson

When troubleshooting a Kubernetes application, always check both the Deployment and the Service.

A correct Deployment image ensures that the application container can start successfully.

A correct Service target port ensures that traffic reaches the application on the port where it is actually listening.

The two important corrections in this challenge were:

1. Fixing the incorrect container image.
2. Changing the Service `targetPort` from `8080` to `5000`.

**Day 64 completed successfully.** 🚀
