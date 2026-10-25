# Day 57 - Print Environment Variable in Kubernetes

## Challenge Overview

This is Day 57 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Kubernetes Environment Variables**. The objective was to identify and print an environment variable configured inside a Kubernetes Pod.

This challenge helped me understand how environment variables are used by containers and how to inspect and retrieve their values using `kubectl`.

### Challenge Requirement

Print the value of the required environment variable from the specified Kubernetes Pod.

The task involves identifying the correct Pod, checking its container configuration, and using Kubernetes commands to print the environment variable's value.

---

## Objectives

The objectives of this challenge are:

- Understand environment variables in Kubernetes.
- Identify the required Pod.
- Inspect Pod and container configuration.
- Identify the required environment variable.
- Print the environment variable's value.
- Use `kubectl get pods` and `kubectl describe pod`.
- Use `kubectl exec` to execute commands inside a container.
- Verify the output of the environment variable.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 57 |
| Topic | Kubernetes Environment Variables |
| Tool | `kubectl` |
| Resource | Pod / Container |
| Task | Print an Environment Variable |
| Status | Completed |

---

# Solution

## Step 1: Check the Kubernetes Pods

First, I listed the Pods in the default namespace.

```bash
kubectl get pods
```

This command displays the available Pods and their current status.

---

## Step 2: Inspect the Required Pod

After identifying the required Pod, I inspected its configuration.

```bash
kubectl describe pod <pod-name>
```

This command displays information about the Pod, including its containers, environment variables, image, and events.

I used this information to identify the required environment variable.

---

## Step 3: Identify the Environment Variable

Environment variables are configured inside a container and contain values that applications can access.

For example:

```text
APP_COLOR=blue
```

In this example:

- `APP_COLOR` is the environment variable name.
- `blue` is the environment variable value.

The actual variable name and value must be taken from the KodeKloud task.

---

## Step 4: Print the Environment Variable

I used `kubectl exec` to run the `printenv` command inside the required container.

```bash
kubectl exec <pod-name> -- printenv <VARIABLE_NAME>
```

Replace `<pod-name>` with the actual Pod name and `<VARIABLE_NAME>` with the required environment variable.

For example:

```bash
kubectl exec my-pod -- printenv APP_COLOR
```

Example output:

```text
blue
```

This confirms that the command successfully printed the variable's value from inside the container.

---

## Step 5: Handle Pods with Multiple Containers

If the Pod contains multiple containers, I can specify the correct container using the `-c` option.

```bash
kubectl exec <pod-name> -c <container-name> -- printenv <VARIABLE_NAME>
```

This ensures that the command runs inside the intended container.

---

## Step 6: Verify the Result

After running the command, I checked the output and confirmed that the environment variable's value was displayed.

The general command format is:

```bash
kubectl exec <pod-name> -- printenv <VARIABLE_NAME>
```

The output should match the value configured in the container.

---

# What I Learned

From this challenge, I learned and practiced:

- What environment variables are.
- How environment variables are used by Kubernetes containers.
- How to list Pods using `kubectl get pods`.
- How to inspect a Pod using `kubectl describe pod`.
- How to execute commands inside containers using `kubectl exec`.
- How to print environment variables using `printenv`.
- How to select a specific container using the `-c` option.
- How to verify the output of a Kubernetes command.
- Why the correct Pod and container names are important when troubleshooting Kubernetes resources.

# Challenge Status

Day 57 — Completed Successfully ✅

**Learning Kubernetes one command at a time.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
