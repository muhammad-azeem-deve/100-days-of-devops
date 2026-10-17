# Day 50 - Create a Kubernetes Pod Using YAML and Set Resource Limits

## Challenge Overview

This is Day 50 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Kubernetes Pods and Resource Management**. The task required creating a Pod using a YAML configuration file and defining CPU and memory resource requests and limits for its container.

### Challenge Requirement

Create a Kubernetes Pod using a YAML file and configure the container's resource requests and limits.

The objective was to define the Pod configuration, deploy it to Kubernetes, and verify that the Pod and its resource settings were configured correctly.

---

## Objectives

The objectives of this challenge are:

- Understand Kubernetes Pods.
- Create a Pod using a YAML file.
- Define a container image.
- Configure CPU and memory requests.
- Configure CPU and memory limits.
- Deploy the Pod using `kubectl apply`.
- Verify the Pod status.
- Inspect resource settings using `kubectl describe`.
- Verify the saved configuration using `kubectl get`.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 50 |
| Technology | Kubernetes |
| Resource Type | Pod |
| Configuration | YAML |
| Example Pod Name | `resource-pod` |
| Example Container Image | `nginx:latest` |
| CPU Request | `100m` |
| Memory Request | `64Mi` |
| CPU Limit | `200m` |
| Memory Limit | `128Mi` |
| Status | Completed |

*Note: The Pod name, image, and resource values above are illustrative examples. Use the exact specifications provided by the lab.*

---

# Solution

## Step 1: Create the YAML File

First, I created a YAML file to define the Kubernetes Pod.

```bash
nano pod.yaml
```

I added the Pod configuration to the file.

## Step 2: Define the Pod Configuration

The example configuration was:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: resource-pod
spec:
  containers:
    - name: nginx-container
      image: nginx:latest
      resources:
        requests:
          cpu: "100m"
          memory: "64Mi"
        limits:
          cpu: "200m"
          memory: "128Mi"
```

### Understanding the Configuration

- `apiVersion`: Specifies the Kubernetes API version.
- `kind`: Defines the resource type as a Pod.
- `metadata.name`: Specifies the Pod name.
- `containers`: Defines the container running inside the Pod.
- `image`: Specifies the container image.
- `resources.requests`: Defines the CPU and memory requested for scheduling.
- `resources.limits`: Defines the maximum CPU and memory available to the container.

## Step 3: Apply the YAML File

After saving the file, I used the following command to create the Pod:

```bash
kubectl apply -f pod.yaml
```

Kubernetes reads the YAML configuration and creates the resource.

## Step 4: Verify the Pod

I checked the Pod status using:

```bash
kubectl get pods
```

This command displays the available Pods and their current status.

The Pod should eventually show `Running` if the container starts successfully.

## Step 5: Verify Resource Requests and Limits

Next, I inspected the Pod configuration:

```bash
kubectl describe pod resource-pod
```

I checked the `Requests` and `Limits` sections to confirm that the CPU and memory values matched the YAML configuration.

## Step 6: Verify the Stored Configuration

I also verified the configuration stored in Kubernetes:

```bash
kubectl get pod resource-pod -o yaml
```

This command displays the Pod's saved configuration, including its resource requests and limits.

---

# Understanding Kubernetes Resource Management

## CPU Resources

CPU resources are commonly measured in millicores.

For example:

```yaml
cpu: "100m"
```

means 100 millicores, or one-tenth of a CPU core.

```yaml
cpu: "200m"
```

means 200 millicores, or one-fifth of a CPU core.

## Memory Resources

Memory can be specified using units such as `Mi` and `Gi`.

For example:

```yaml
memory: "64Mi"
```

requests 64 mebibytes of memory.

```yaml
memory: "128Mi"
```

sets a limit of 128 mebibytes of memory.

## Requests vs. Limits

| Resource Setting | Purpose |
|------|---------|
| Requests | Helps Kubernetes schedule the Pod onto a suitable node. |
| CPU limits | Restricts CPU consumption; exceeding the limit causes throttling. |
| Memory limits | Restricts memory consumption; exceeding the limit can cause an OOM termination. |

---

# Verification Commands

The main commands used to deploy and inspect the Pod were:

```bash
kubectl apply -f pod.yaml
kubectl get pods
kubectl describe pod resource-pod
kubectl get pod resource-pod -o yaml
```

These commands help verify that the Pod exists, inspect its status, and check its configured resources.

---

# What I Learned

From this challenge, I learned and practiced:

- How to create a Kubernetes Pod using YAML.
- How to define a container image in a Pod specification.
- How to configure CPU and memory requests.
- How to configure CPU and memory limits.
- How to deploy resources using `kubectl apply`.
- How to verify Pod status using `kubectl get pods`.
- How to inspect resource settings using `kubectl describe`.
- How to view the saved Pod specification using `kubectl get -o yaml`.
- Why resource management is important in Kubernetes.
- The difference between resource requests and limits.

# Challenge Status

Day 50 — Completed Successfully ✅

**Created a Kubernetes Pod using YAML and learned how to configure CPU and memory resource requests and limits.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
