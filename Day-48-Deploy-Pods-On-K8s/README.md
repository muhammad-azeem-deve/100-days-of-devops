# Day 48 - Deploy Pods in Kubernetes Clusters

## Challenge Overview

This is Day 48 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Kubernetes Pod Deployment**. The objective was to create and deploy a Pod in a Kubernetes cluster and verify that it was configured correctly.

A Pod is the smallest deployable unit in Kubernetes. It contains one or more containers that run an application.

### Challenge Requirement

Deploy a Pod in the Kubernetes cluster using the required Pod name and container image.

For this walkthrough, the example configuration is:

- Pod Name: `pod-nginx`
- Container Image: `nginx:latest`
- Resource: Kubernetes Pod

**Note:** Replace the example values if your actual KodeKloud task specifies different requirements.

---

## Objectives

The objectives of this challenge are:

- Access the Kubernetes server.
- Verify that the Kubernetes cluster is accessible.
- Understand the purpose of a Kubernetes Pod.
- Create a Pod using `kubectl`.
- Specify the required container image.
- Verify the Pod status.
- Inspect the Pod configuration.
- Understand basic Kubernetes troubleshooting commands.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 48 |
| Technology | Kubernetes |
| Resource | Pod |
| Example Pod Name | `pod-nginx` |
| Example Image | `nginx:latest` |
| Tool | `kubectl` |

---

# Solution

## Step 1: Access the Kubernetes Server

First, I accessed the server where Kubernetes was configured.

```bash
ssh <username>@<server-hostname>
```

## Step 2: Verify Kubernetes Nodes

I checked the Kubernetes nodes using:

```bash
kubectl get nodes
```

This command displays the nodes registered in the cluster.

## Step 3: Create the Pod

I created a Pod named `pod-nginx` using the Nginx container image.

```bash
kubectl run pod-nginx --image=nginx:latest
```

The command creates a Pod with the specified name and image in the current namespace, which is normally `default` unless configured otherwise.

## Step 4: Verify the Pod

After creating the Pod, I checked its status:

```bash
kubectl get pods
```

To inspect more information, I used:

```bash
kubectl describe pod pod-nginx
```

The Pod should eventually reach the `Running` state if the image is available and the container starts successfully.

## Step 5: Verify the Container Image

I verified the image configured for the Pod:

```bash
kubectl get pod pod-nginx -o jsonpath='{.spec.containers[*].image}'
```

Expected output:

```text
nginx:latest
```

---

# Understanding Kubernetes Pods

A Pod provides an environment in which containers run.

For example:

```text
Kubernetes Cluster
       |
       v
   pod-nginx
       |
       v
  Nginx Container
       |
       v
  Nginx Application
```

The Pod is managed by Kubernetes, while the container runs the application.

---

# Troubleshooting

If the Pod does not start successfully, I can use the following commands.

### Check Pod Status

```bash
kubectl get pods
```

### Inspect Pod Events

```bash
kubectl describe pod pod-nginx
```

### View Container Logs

```bash
kubectl logs pod-nginx
```

These commands help identify common problems, such as image-pull failures, scheduling issues, or container startup errors.

---

# What I Learned

From this challenge, I learned and practiced:

- The basic concept of Kubernetes Pods.
- How to check Kubernetes nodes.
- How to create a Pod using `kubectl run`.
- How to specify a container image.
- How to verify Pod status using `kubectl get pods`.
- How to inspect a Pod using `kubectl describe`.
- How to verify the configured container image.
- How to investigate Pod startup problems using logs and events.

# Challenge Status

Day 48 — Pod Deployment Practice

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
