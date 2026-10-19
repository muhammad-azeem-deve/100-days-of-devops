# Day 52 - Roll Back a Kubernetes Deployment

## Challenge Overview

This is Day 52 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Kubernetes Deployment Rollbacks**. An Nginx application was running with its latest version, but a customer reported a bug in the current version. The DevOps team needed to restore the previous working version of the application.

To solve this task, I used Kubernetes rollout commands to inspect the Deployment history, roll back to the previous revision, and verify the status of the Deployment.

### Challenge Requirement

Roll back the Nginx Deployment to its previous version because the latest version contains a bug.

The objective was to use Kubernetes Deployment rollback functionality instead of manually changing the application version.

---

## Objectives

The objectives of this challenge are:

- Understand Kubernetes Deployments.
- Check the existing Deployments in the cluster.
- Inspect Deployment rollout history.
- Roll back a Deployment to its previous revision.
- Verify the rollout status.
- Check the status of the Deployment and its Pods.
- Understand how Kubernetes manages Deployment revisions.
- Learn how to recover from a problematic application update.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 52 |
| Technology | Kubernetes |
| Application | Nginx |
| Task | Deployment Rollback |
| Main Command | `kubectl rollout undo` |
| Objective | Restore the previous Deployment revision |
| Status | Completed |

---

# Solution

## Step 1: Check Existing Deployments

First, I checked the Deployments available in the Kubernetes cluster.

I used:

```bash
kubectl get deployments
```

This command displays the Deployments in the current namespace.

I identified the Nginx Deployment that needed to be rolled back.

## Step 2: Check Deployment Rollout History

Next, I inspected the rollout history of the Deployment.

```bash
kubectl rollout history deployment <deployment-name>
```

This command displays the available Deployment revisions.

Kubernetes maintains rollout history so that previous Deployment revisions can be restored when necessary.

## Step 3: Roll Back the Deployment

After identifying the Deployment, I used the rollback command:

```bash
kubectl rollout undo deployment <deployment-name>
```

This command rolls back the Deployment to its previous revision.

For example, if the Deployment is named `nginx-deployment`, the command would be:

```bash
kubectl rollout undo deployment nginx-deployment
```

The actual Deployment name must match the name provided in the lab.

## Step 4: Verify Rollout Status

After initiating the rollback, I checked whether the rollout completed successfully.

```bash
kubectl rollout status deployment <deployment-name>
```

This command waits for the Deployment rollout to complete and reports its status.

A successful rollout indicates that Kubernetes has completed the Deployment update according to its rollout strategy.

## Step 5: Verify the Deployment

I checked the Deployment again:

```bash
kubectl get deployments
```

This helped me verify the Deployment's replica and availability status.

## Step 6: Verify the Pods

I checked the Pods in the current namespace:

```bash
kubectl get pods
```

This helped me confirm that the application Pods were running after the rollback.

## Step 7: Check Rollout History Again

Finally, I checked the rollout history again:

```bash
kubectl rollout history deployment <deployment-name>
```

This allowed me to inspect the Deployment revisions after the rollback.

---

# Understanding Kubernetes Rollback

A Deployment rollback restores a previous revision of a Kubernetes Deployment.

For example:

```text
Revision 1: Previous working version
       |
       v
Revision 2: Latest version with a bug
       |
       v
Customer reports a problem
       |
       v
kubectl rollout undo deployment <deployment-name>
       |
       v
Previous revision restored
```

Kubernetes manages the Deployment and its ReplicaSets to perform the rollback.

A rollback does not necessarily mean that Kubernetes restores the exact previous application data or reverses database changes. It restores the previous Deployment revision.

---

# Important Commands

### List Deployments

```bash
kubectl get deployments
```

### View Rollout History

```bash
kubectl rollout history deployment <deployment-name>
```

### Roll Back to the Previous Revision

```bash
kubectl rollout undo deployment <deployment-name>
```

### Check Rollout Status

```bash
kubectl rollout status deployment <deployment-name>
```

### Check Pods

```bash
kubectl get pods
```

---

# What I Learned

From this challenge, I learned and practiced:

- How Kubernetes Deployments manage application versions.
- How to list Deployments using `kubectl get deployments`.
- How to inspect rollout history.
- How to roll back a Deployment to its previous revision.
- How to use `kubectl rollout undo`.
- How to monitor a Deployment rollout.
- How to verify the status of application Pods.
- How Kubernetes can help recover from a faulty application update.
- Why rollback is an important part of application deployment and release management.

# Challenge Status

Day 52 — Completed Successfully 

**A faulty application update can cause problems for customers. Kubernetes rollback helps the DevOps team restore a previous Deployment revision quickly.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
