# Day 51 - Execute Rolling Updates in Kubernetes

## Challenge Overview

This is Day 51 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Kubernetes Rolling Updates**. The task required updating the NGINX container image in an existing Kubernetes Deployment named `nginx`.

A rolling update allows Kubernetes to gradually replace old Pods with new Pods instead of replacing all application instances at once.

### Challenge Requirement

Update the container image of the existing Deployment named `nginx` to the new NGINX version specified in the lab.

The objective was to update the image, monitor the rollout, and verify that the Deployment was successfully updated.

---

## Objectives

The objectives of this challenge are:

- Understand Kubernetes Deployments.
- Identify the existing `nginx` Deployment.
- Check the current container image.
- Update the container image to a new version.
- Execute a rolling update.
- Monitor the rollout status.
- Verify the updated Deployment and Pods.
- Understand the difference between a rolling update and a rollback.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 51 |
| Technology | Kubernetes |
| Resource | Deployment |
| Deployment Name | `nginx` |
| Container Image | NGINX |
| Operation | Rolling Update |
| Status | Completed |

---

# Solution

## Step 1: Check the Existing Deployment

First, I checked the existing Kubernetes Deployments:

```bash
kubectl get deployments
```

I identified the Deployment named `nginx`.

---

## Step 2: Inspect the Deployment

Next, I checked the Deployment details:

```bash
kubectl describe deployment nginx
```

I inspected the container image to identify the currently configured NGINX version.

---

## Step 3: Update the NGINX Image

I updated the Deployment's container image using the `kubectl set image` command.

```bash
kubectl set image deployment/nginx nginx=nginx:<new-version>
```

Here:

- `kubectl set image` updates the container image.
- `deployment/nginx` identifies the Deployment.
- `nginx` is the container name, assuming it matches the Deployment's container name.
- `nginx:<new-version>` specifies the required NGINX image version.

**Note:** Replace `<new-version>` with the exact version specified in the KodeKloud lab.

---

## Step 4: Monitor the Rolling Update

After updating the image, I monitored the rollout:

```bash
kubectl rollout status deployment/nginx
```

This command waits for Kubernetes to complete the rollout.

A successful rollout indicates that the Deployment has progressed successfully according to its rollout conditions.

---

## Step 5: Verify the Deployment

I checked the Deployment status:

```bash
kubectl get deployment nginx
```

I also inspected its configuration:

```bash
kubectl describe deployment nginx
```

These commands helped verify that the updated container image was configured.

---

## Step 6: Verify the Pods

Finally, I checked the Pods:

```bash
kubectl get pods
```

I verified that the Pods were running and that the Deployment had completed its update.

---

# Rolling Update vs Rollback

A rolling update and a rollback are different operations.

### Rolling Update

A rolling update changes the Deployment to use a new container image.

```bash
kubectl set image deployment/nginx nginx=nginx:<new-version>
```

### Rollback

A rollback returns the Deployment to its previous revision.

```bash
kubectl rollout undo deployment/nginx
```

For this challenge, the objective was to update the NGINX container to a new version, so the rolling update command was the relevant operation.

---

# What I Learned

From this challenge, I learned and practiced:

- How Kubernetes Deployments manage application Pods.
- How to inspect a Deployment using `kubectl describe`.
- How to update a container image using `kubectl set image`.
- How rolling updates gradually replace application Pods.
- How to monitor updates using `kubectl rollout status`.
- How to verify Deployments and Pods.
- The difference between updating an application and rolling back a previous revision.
- How Kubernetes supports application updates with minimal disruption.

# Challenge Status

Day 51 — Completed Successfully ✅

**Kubernetes Rolling Updates — updating container images and verifying Deployment health.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
