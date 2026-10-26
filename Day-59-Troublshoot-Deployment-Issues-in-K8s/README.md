# Day 59 - Troubleshoot Deployment Issues in Kubernetes

## Challenge Overview

This is Day 59 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Kubernetes Deployment Troubleshooting**. The task required identifying and fixing issues that prevented a Kubernetes deployment from working correctly.

During the troubleshooting process, I identified two configuration problems:

1. An incorrect container image name caused by a spelling mistake.
2. An incorrect ConfigMap reference in the Deployment configuration.

The objective was to correct these issues and verify that the Kubernetes Deployment and its Pods were working properly.

---

## Challenge Requirement

Troubleshoot the Kubernetes Deployment by correcting the image name and ConfigMap reference.

The task involved:

- Inspecting the existing Kubernetes Pods and Deployments.
- Identifying the incorrect container image name.
- Correcting the image name.
- Identifying the incorrect ConfigMap reference.
- Updating the Deployment configuration.
- Verifying the Deployment rollout.
- Confirming that the Pods were running successfully.

---

## Objectives

The objectives of this challenge are:

- Understand Kubernetes Deployment troubleshooting.
- Inspect Pods and Deployments using `kubectl`.
- Identify image pull errors.
- Correct spelling mistakes in container image names.
- Understand ConfigMaps and their references.
- Update Deployment configurations.
- Monitor Deployment rollouts.
- Verify Pod readiness and application status.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 59 |
| Technology | Kubernetes |
| Resource | Deployment |
| Issues | Incorrect image name and ConfigMap reference |
| Tools | `kubectl` |
| Status | Completed |

---

# Solution

## Step 1: Check the Kubernetes Pods

First, I checked the Pods to identify whether any containers were failing to start.

```bash
kubectl get pods
```

I looked for statuses such as:

- `ImagePullBackOff`
- `ErrImagePull`
- `CreateContainerConfigError`
- `CrashLoopBackOff`

These statuses can indicate different problems. For example, an incorrect image name can cause an image pull error, while an invalid ConfigMap reference can prevent a container from starting.

## Step 2: Check the Deployments

I listed the existing Deployments:

```bash
kubectl get deployments
```

Then I inspected the affected Deployment:

```bash
kubectl describe deployment <deployment-name>
```

This helped me inspect the container image, configuration references, and related events.

## Step 3: Identify and Correct the Image Name

I checked the container image name in the Deployment configuration.

The image name contained a spelling mistake. For example:

```text
Incorrect: aplin
Correct:   alpine
```

The actual image name and tag must match the values required by the task.

I edited the Deployment:

```bash
kubectl edit deployment <deployment-name>
```

I corrected the `image` field and saved the changes.

## Step 4: Identify and Correct the ConfigMap Reference

Next, I checked the available ConfigMaps:

```bash
kubectl get configmaps
```

I inspected the Deployment configuration to identify the incorrect ConfigMap reference.

For example, a container might use a ConfigMap through `envFrom`:

```yaml
envFrom:
  - configMapRef:
      name: <correct-configmap-name>
```

A ConfigMap can also be referenced through `configMapKeyRef` or a volume.

I corrected the reference to match the ConfigMap required by the challenge and saved the updated configuration.

**Important:** The example above is illustrative. The actual ConfigMap name and reference type depend on the Kubernetes resources provided in the lab.

## Step 5: Verify the Deployment Rollout

After correcting both issues, I checked the Deployment:

```bash
kubectl get deployments
```

I monitored the rollout:

```bash
kubectl rollout status deployment/<deployment-name>
```

A successful rollout indicates that the Deployment has completed its update successfully.

## Step 6: Verify the Pods

Finally, I checked the Pods again:

```bash
kubectl get pods
```

I also inspected the Deployment if further verification was needed:

```bash
kubectl describe deployment <deployment-name>
```

The objective was to ensure that the Pods became ready and that the Deployment was functioning as expected.

---

# Troubleshooting Summary

The troubleshooting process followed this sequence:

1. Check the Pods.
2. Inspect the Deployment.
3. Identify the incorrect image name.
4. Correct the image name.
5. Identify the incorrect ConfigMap reference.
6. Correct the ConfigMap reference.
7. Monitor the Deployment rollout.
8. Verify the final Pod status.

---

# What I Learned

From this challenge, I learned and practiced:

- How to troubleshoot Kubernetes Deployments.
- How to inspect Pod and Deployment status.
- How to identify image pull problems.
- How a spelling mistake in an image name can prevent a container from starting.
- How to inspect and update a Deployment using `kubectl edit`.
- How Kubernetes ConfigMaps provide configuration to applications.
- How an incorrect ConfigMap reference can prevent a Pod from starting correctly.
- How to monitor a Deployment rollout.
- How to verify the health and readiness of Kubernetes Pods.
- Why checking events and resource descriptions is important during troubleshooting.

# Challenge Status

Day 59 — Completed Successfully ✅

**Two configuration issues identified, corrected, and verified as part of my Kubernetes troubleshooting practice.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
