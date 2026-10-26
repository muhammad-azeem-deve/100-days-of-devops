# Day 59 - Commands

This file contains the commands used during **Day 59 of my 100 Days of DevOps Challenge by KodeKloud**.

The objective was to troubleshoot Kubernetes Deployment issues caused by an incorrect container image name and an incorrect ConfigMap reference.

---

## Step 1: Check Kubernetes Pods

```bash
kubectl get pods
```

Check Pods across all namespaces if required:

```bash
kubectl get pods -A
```

Look for statuses such as `ImagePullBackOff`, `ErrImagePull`, or `CreateContainerConfigError`.

---

## Step 2: List Deployments

```bash
kubectl get deployments
```

List Deployments across all namespaces if required:

```bash
kubectl get deployments -A
```

---

## Step 3: Inspect the Affected Deployment

```bash
kubectl describe deployment <deployment-name>
```

Check the image name, ConfigMap references, and events.

---

## Step 4: Inspect Pod Details

```bash
kubectl describe pod <pod-name>
```

This provides information about container status, events, image pull errors, and configuration problems.

---

## Step 5: Check Container Image Names

Inspect the Deployment YAML:

```bash
kubectl get deployment <deployment-name> -o yaml
```

Find the container image field:

```yaml
containers:
  - name: <container-name>
    image: <image-name>:<tag>
```

Check for spelling mistakes and verify the expected image name and tag.

Example:

```text
Incorrect: aplin
Correct:   alpine
```

The example is illustrative; use the actual image name required by the challenge.

---

## Step 6: Edit the Deployment

```bash
kubectl edit deployment <deployment-name>
```

Correct the image field and the ConfigMap reference.

Save the changes and exit the editor.

---

## Step 7: List ConfigMaps

```bash
kubectl get configmaps
```

For all namespaces, if required:

```bash
kubectl get configmaps -A
```

Inspect a particular ConfigMap:

```bash
kubectl describe configmap <configmap-name>
```

---

## Step 8: Check ConfigMap References

Inspect the Deployment:

```bash
kubectl get deployment <deployment-name> -o yaml
```

Look for references such as:

```yaml
envFrom:
  - configMapRef:
      name: <correct-configmap-name>
```

Other possible references include `configMapKeyRef` and ConfigMap volumes.

Ensure the Deployment references the correct ConfigMap for the task.

---

## Step 9: Monitor the Deployment Rollout

```bash
kubectl rollout status deployment/<deployment-name>
```

Check the Deployment again:

```bash
kubectl get deployments
```

---

## Step 10: Verify Pods

```bash
kubectl get pods
```

Check individual Pod details if needed:

```bash
kubectl describe pod <pod-name>
```

---

# Quick Command Summary

```bash
# Check Pods
kubectl get pods

# List Deployments
kubectl get deployments

# Inspect Deployment
kubectl describe deployment <deployment-name>

# Inspect Pod
kubectl describe pod <pod-name>

# View Deployment YAML
kubectl get deployment <deployment-name> -o yaml

# List ConfigMaps
kubectl get configmaps

# Inspect ConfigMap
kubectl describe configmap <configmap-name>

# Edit Deployment
kubectl edit deployment <deployment-name>

# Monitor rollout
kubectl rollout status deployment/<deployment-name>

# Verify final status
kubectl get deployments
kubectl get pods
```

---

# Important Lessons

- Always inspect Pod events before changing Kubernetes resources.
- Check image names carefully because spelling mistakes can cause image pull failures.
- Verify ConfigMap names against the available resources and task requirements.
- Use `kubectl describe` to investigate problems.
- Use `kubectl edit` to correct Deployment configuration.
- Use `kubectl rollout status` to monitor whether the updated Deployment becomes ready.
- Do not assume that every Pod failure is caused by an image issue; inspect the error messages to identify the actual cause.

**Day 59 completed successfully.** 🚀
