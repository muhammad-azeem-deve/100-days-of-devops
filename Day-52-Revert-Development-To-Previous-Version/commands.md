# Day 52 - Commands

This file contains the commands used during **Day 52 of my 100 Days of DevOps Challenge by KodeKloud**.

The objective was to roll back an Nginx Deployment to its previous revision because the latest version contained a bug.

---

## Step 1: Check Deployments

```bash
kubectl get deployments
```

This lists the Deployments in the current namespace.

---

## Step 2: Check Deployment Rollout History

```bash
kubectl rollout history deployment <deployment-name>
```

This displays the available revisions of the Deployment.

---

## Step 3: Roll Back to the Previous Revision

```bash
kubectl rollout undo deployment <deployment-name>
```

Example:

```bash
kubectl rollout undo deployment nginx-deployment
```

Replace `nginx-deployment` with the actual Deployment name from the lab.

---

## Step 4: Verify Rollout Status

```bash
kubectl rollout status deployment <deployment-name>
```

This checks whether the Deployment rollout has completed successfully.

---

## Step 5: Check Deployment Status

```bash
kubectl get deployments
```

This displays the current state of the Deployment and its replicas.

---

## Step 6: Check Pods

```bash
kubectl get pods
```

This lists the Pods in the current namespace so that their status can be checked.

---

## Step 7: Check Rollout History Again

```bash
kubectl rollout history deployment <deployment-name>
```

This displays the rollout history after the rollback.

---

# Quick Command Summary

```bash
# List Deployments
kubectl get deployments

# Check Deployment history
kubectl rollout history deployment <deployment-name>

# Roll back to the previous revision
kubectl rollout undo deployment <deployment-name>

# Verify rollout completion
kubectl rollout status deployment <deployment-name>

# Check Deployment status
kubectl get deployments

# Check Pods
kubectl get pods

# Inspect rollout history again
kubectl rollout history deployment <deployment-name>
```

---

# Optional: Roll Back to a Specific Revision

If the task requires restoring a particular revision rather than the immediately previous one, first inspect the rollout history:

```bash
kubectl rollout history deployment <deployment-name>
```

Then specify the required revision:

```bash
kubectl rollout undo deployment <deployment-name> --to-revision=<revision-number>
```

Use this only when the task specifies a particular revision.

---

# Important Lesson

The most important command in this challenge was:

```bash
kubectl rollout undo deployment <deployment-name>
```

It tells Kubernetes to roll back a Deployment to its previous revision.

Always verify the result using:

```bash
kubectl rollout status deployment <deployment-name>
kubectl get deployments
kubectl get pods
```

**Day 52 completed successfully.** 
