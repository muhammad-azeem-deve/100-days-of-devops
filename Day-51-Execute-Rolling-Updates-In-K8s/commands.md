# Day 51 - Commands

This file contains the commands used during **Day 51 of my 100 Days of DevOps Challenge by KodeKloud**.

The objective was to execute a rolling update on the Kubernetes Deployment named `nginx` by updating its container image.

---

## Step 1: Check Deployments

```bash
kubectl get deployments
```

List the Deployments and identify `nginx`.

---

## Step 2: Inspect the Deployment

```bash
kubectl describe deployment nginx
```

Check the container name and current image.

---

## Step 3: Update the NGINX Container Image

```bash
kubectl set image deployment/nginx nginx=nginx:<new-version>
```

Replace `<new-version>` with the exact NGINX version specified in the lab.

If the container name is different, replace the second `nginx` with the actual container name.

---

## Step 4: Monitor the Rollout

```bash
kubectl rollout status deployment/nginx
```

Wait for the rollout to complete.

---

## Step 5: Verify the Deployment

```bash
kubectl get deployment nginx
```

Inspect the updated image:

```bash
kubectl describe deployment nginx
```

---

## Step 6: Verify the Pods

```bash
kubectl get pods
```

Check that the Pods are running and that the Deployment has completed its update.

---

## Step 7: Check Rollout History

```bash
kubectl rollout history deployment/nginx
```

This displays the available Deployment revisions.

---

## Optional: Roll Back to the Previous Revision

If a rolling update must be reversed, use:

```bash
kubectl rollout undo deployment/nginx
```

Check the rollback status:

```bash
kubectl rollout status deployment/nginx
```

**Important:** Do not run the rollback command when the lab requires updating to a new NGINX version.

---

# Quick Command Summary

```bash
# List Deployments
kubectl get deployments

# Inspect the nginx Deployment
kubectl describe deployment nginx

# Update the container image
kubectl set image deployment/nginx nginx=nginx:<new-version>

# Monitor the rolling update
kubectl rollout status deployment/nginx

# Verify Deployment status
kubectl get deployment nginx

# Inspect the updated image
kubectl describe deployment nginx

# Verify Pods
kubectl get pods

# Check rollout history
kubectl rollout history deployment/nginx
```

---

# Important Lesson

The main command for this challenge was:

```bash
kubectl set image deployment/nginx nginx=nginx:<new-version>
```

This updates the Deployment to use the specified NGINX image version.

The rollout can then be monitored using:

```bash
kubectl rollout status deployment/nginx
```

Always use the exact image version required by the lab, and verify the container name before applying the update.

**Day 51 completed successfully.**
