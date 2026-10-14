# Day 49 - Deploy Application Using Kubernetes Deployments

## Challenge Overview

This is Day 49 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Kubernetes Deployments** and involved deploying an application on a Kubernetes cluster.

A Deployment manages application Pods and helps maintain the desired number of running replicas. It also supports application updates and rollbacks.

### Challenge Requirement

Deploy an application using a Kubernetes Deployment and verify that the Deployment and its Pods are running successfully.

For practice, this documentation uses the following example:

- Deployment Name: `nginx-deployment`
- Container Image: `nginx`
- Replicas: `3`

Use the exact values provided in the KodeKloud task if they are different.

---

## Objectives

The objectives of this challenge are:

- Check the Kubernetes cluster.
- Create a Kubernetes Deployment.
- Deploy an application using a container image.
- Configure the required number of replicas.
- Verify the Deployment status.
- Verify that the Pods are running.
- Check the rollout status.
- Understand how Kubernetes Deployments manage applications.

---

## Environment

| Item | Details |
|---|---|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 49 |
| Technology | Kubernetes |
| Resource | Deployment |
| Example Deployment | `nginx-deployment` |
| Example Image | `nginx` |
| Example Replicas | 3 |
| Status | Completed after lab verification |

---

# Solution

## Step 1: Check Kubernetes Nodes

First, I checked the Kubernetes cluster to verify that the nodes were available.

```bash
kubectl get nodes
```

This command displays the nodes registered with the Kubernetes cluster.

## Step 2: Create the Deployment

Next, I created a Deployment using the NGINX container image with three replicas.

```bash
kubectl create deployment nginx-deployment --image=nginx --replicas=3
```

### Explanation

- `kubectl` is the Kubernetes command-line tool.
- `create deployment` creates a Deployment.
- `nginx-deployment` is the Deployment name.
- `--image=nginx` specifies the container image.
- `--replicas=3` requests three application Pods.

## Step 3: Verify the Deployment

After creating the Deployment, I checked its status.

```bash
kubectl get deployments
```

The Deployment should eventually show:

- `READY`: 3/3
- `UP-TO-DATE`: 3
- `AVAILABLE`: 3

These values are expected once all three replicas are ready.

## Step 4: Verify the Pods

I checked the application Pods using:

```bash
kubectl get pods
```

Kubernetes should create three Pods managed by the Deployment.

The Pods may initially show `ContainerCreating` before reaching `Running`.

## Step 5: Check Rollout Status

I verified that the Deployment rollout completed successfully.

```bash
kubectl rollout status deployment/nginx-deployment
```

A successful rollout confirms that Kubernetes has completed the Deployment rollout.

## Step 6: Inspect Deployment Details

I used the following command to inspect the Deployment:

```bash
kubectl describe deployment nginx-deployment
```

This displays details such as the image, replica count, labels, events, and related ReplicaSets.

---

# Understanding Kubernetes Deployments

A Deployment manages application replicas through a ReplicaSet.

The basic structure is:

```text
Kubernetes Deployment
        |
        v
    ReplicaSet
        |
   +----+----+
   |    |    |
  Pod1 Pod2 Pod3
```

If a Pod fails or is deleted, Kubernetes attempts to create a replacement to maintain the desired replica count.

## What I Learned

From this challenge, I learned and practiced:

- How to check Kubernetes nodes.
- How to create a Deployment using `kubectl`.
- How to specify a container image.
- How to configure application replicas.
- How to check Deployments using `kubectl get deployments`.
- How to inspect Pods using `kubectl get pods`.
- How to monitor a Deployment rollout.
- How Deployments and ReplicaSets manage Pods.
- Why Kubernetes is useful for maintaining application availability.

# Challenge Status

Day 49 — Completed after lab verification 

**100 Days. 100 Challenges. One DevOps Journey.** 🚀

I will continue documenting each challenge.
