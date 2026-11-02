# Day 64 - Fix Python App Deployed on Kubernetes Cluster

## Challenge Overview

This is Day 64 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Kubernetes Troubleshooting** and required fixing a Python application that was already deployed on a Kubernetes cluster but was not working correctly.

The application had configuration issues in its Kubernetes Deployment and Service.

The two main problems were:

1. Incorrect container image in the Deployment.
2. Incorrect target port in the Kubernetes Service.

The objective was to identify these issues, correct the configurations, and make the Python application accessible through the specified NodePort.

### Challenge Requirement

Fix the existing Python application deployed on the Kubernetes cluster.

The requirements included:

- Correct the container image in the Deployment.
- Configure the correct target port for the Python Flask application.
- Keep the required NodePort configuration.
- Verify that the Deployment and Pod are running successfully.
- Verify the Service configuration.
- Test application accessibility through the NodePort.

---

## Objectives

The objectives of this challenge are:

- Understand how to troubleshoot Kubernetes applications.
- Inspect existing Deployments, Pods, and Services.
- Identify incorrect container image configurations.
- Fix the Deployment image.
- Understand the difference between Service port, targetPort, and NodePort.
- Correct the Service target port.
- Verify Deployment rollout status.
- Check Pod health and status.
- Verify Service endpoints.
- Access the application through the NodePort.
- Understand how Kubernetes Services communicate with Pods.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 64 |
| Technology | Kubernetes |
| Application | Python Flask App |
| Deployment | `python-deployment-devops` |
| Service | `python-service-devops` |
| Correct Application Port | `5000` |
| Service Port | `8080` |
| NodePort | `32345` |
| Container Image | Correct image specified by the challenge |
| Status | Completed |

---

# Solution

## Step 1: Check Kubernetes Cluster

First, I accessed the jump host where `kubectl` was already configured to communicate with the Kubernetes cluster.

I checked the available Kubernetes nodes:

```bash
kubectl get nodes
```

Then I checked the existing Pods, Deployments, and Services:

```bash
kubectl get pods
kubectl get deployments
kubectl get services
```

This helped me understand the existing application resources.

---

## Step 2: Check Deployment Status

Next, I checked the status of the Python application Deployment:

```bash
kubectl get deployment python-deployment-devops
```

The Deployment was not in a healthy state.

For example, the output could show:

```text
NAME                       READY   UP-TO-DATE   AVAILABLE
python-deployment-devops   0/1     1            0
```

This indicated that the application was not ready.

I then checked the Pods:

```bash
kubectl get pods
```

To investigate the issue in more detail, I described the application Pod:

```bash
kubectl describe pod <pod-name>
```

During troubleshooting, I identified an image-related issue.

The Pod could show errors such as:

```text
ImagePullBackOff
ErrImagePull
```

These errors indicate that Kubernetes is unable to pull the configured container image successfully.

---

## Step 3: Fix the Incorrect Container Image

After identifying the image issue, I opened the Deployment configuration:

```bash
kubectl edit deployment python-deployment-devops
```

I located the container image configuration:

```yaml
containers:
  - name: python-container-devops
    image: wrong-image
```

I replaced the incorrect image with the correct image specified in the KodeKloud challenge.

For the commonly documented version of this task, the correct image is:

```yaml
image: poroko/flask-demo-app
```

I saved the changes and exited the editor.

Kubernetes then updated the Deployment and created a new Pod using the corrected image.

---

## Step 4: Verify Deployment Rollout

After updating the image, I checked the Deployment rollout:

```bash
kubectl rollout status deployment/python-deployment-devops
```

Then I checked the Deployment again:

```bash
kubectl get deployment
```

I also verified the Pod status:

```bash
kubectl get pods
```

The expected result was:

```text
READY   STATUS
1/1     Running
```

This confirmed that the application container had started successfully.

---

## Step 5: Check the Kubernetes Service

After fixing the Deployment, I checked the existing Kubernetes Services:

```bash
kubectl get svc
```

I inspected the Python application Service:

```bash
kubectl describe svc python-service-devops
```

The Service configuration showed:

```text
Port:        8080/TCP
TargetPort:  8080/TCP
NodePort:    32345/TCP
```

Here, I identified the second issue.

The Python Flask application listens on port `5000`, but the Service was forwarding traffic to port `8080`.

Therefore, the target port needed to be corrected.

---

## Step 6: Fix the Service Target Port

I opened the Service configuration:

```bash
kubectl edit svc python-service-devops
```

I located the following configuration:

```yaml
ports:
  - port: 8080
    targetPort: 8080
    nodePort: 32345
```

I changed the target port from `8080` to `5000`.

The corrected configuration was:

```yaml
ports:
  - port: 8080
    targetPort: 5000
    nodePort: 32345
```

I saved the changes and exited the editor.

The final configuration was:

- Service Port: `8080`
- Target Port: `5000`
- NodePort: `32345`

The NodePort remained unchanged.

---

## Step 7: Verify the Service Configuration

After updating the Service, I checked its configuration:

```bash
kubectl get svc python-service-devops
```

Then I inspected the detailed Service information:

```bash
kubectl describe svc python-service-devops
```

The expected port configuration was:

```text
Port:        8080/TCP
TargetPort:  5000/TCP
NodePort:    32345/TCP
```

This confirmed that the Service was forwarding requests to the correct application port.

---

## Step 8: Verify Service Endpoints

Next, I verified that the Service was correctly connected to the application Pod:

```bash
kubectl get endpoints python-service-devops
```

The endpoint should point to the Pod IP with port `5000`.

This confirmed that the Service had a backend endpoint available.

---

## Step 9: Test Application Accessibility

Finally, I tested the Python application through the NodePort.

The command format was:

```bash
curl http://<node-ip>:32345
```

The NodePort `32345` provides external access to the application through a Kubernetes node.

A successful HTTP response confirmed that the application was accessible.

---

# Understanding Kubernetes Ports

Kubernetes Services use three important port configurations.

### 1. Port

The port exposed by the Kubernetes Service.

```yaml
port: 8080
```

### 2. TargetPort

The port where the application is listening inside the Pod.

```yaml
targetPort: 5000
```

### 3. NodePort

The port exposed on the Kubernetes nodes for external access.

```yaml
nodePort: 32345
```

### Final Traffic Flow

```text
External User
      |
      |
      v
NodePort: 32345
      |
      |
      v
Service Port: 8080
      |
      |
      v
TargetPort: 5000
      |
      |
      v
Python Flask Application
```

---

# What I Learned

From this challenge, I learned and practiced:

- How to troubleshoot applications deployed on Kubernetes.
- How to inspect Deployments, Pods, and Services.
- How to identify `ImagePullBackOff` and `ErrImagePull` errors.
- How to correct an incorrect container image.
- How to update a Deployment using `kubectl edit`.
- How to verify Deployment rollout status.
- How Kubernetes Services route traffic to Pods.
- The difference between `port`, `targetPort`, and `nodePort`.
- How to correct a Service target port.
- How to inspect Service endpoints.
- How to test application accessibility using `curl`.
- The importance of checking both Deployment and Service configurations when troubleshooting Kubernetes applications.

# Challenge Status

Day 64 — Completed Successfully ✅

**Fixed the incorrect container image and Service target port, verified the Kubernetes resources, and restored application accessibility.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
