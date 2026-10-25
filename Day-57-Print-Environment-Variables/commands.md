# Day 57 - Commands

This file contains the commands used during **Day 57 of my 100 Days of DevOps Challenge by KodeKloud**.

The objective was to print the value of an environment variable configured inside a Kubernetes container.

---

## Step 1: List the Pods

```bash
kubectl get pods
```

This command lists Pods in the current namespace.

To list Pods across all namespaces:

```bash
kubectl get pods -A
```

---

## Step 2: Inspect the Pod

```bash
kubectl describe pod <pod-name>
```

This command displays the Pod's configuration and container details.

---

## Step 3: Check the Environment Variable

To inspect environment variables in the Pod configuration, use:

```bash
kubectl describe pod <pod-name>
```

Look for the `Environment` section in the output.

---

## Step 4: Print the Environment Variable

Use `kubectl exec` with `printenv`:

```bash
kubectl exec <pod-name> -- printenv <VARIABLE_NAME>
```

Example:

```bash
kubectl exec my-pod -- printenv APP_COLOR
```

Example output:

```text
blue
```

The Pod name and variable name above are examples. Use the actual values provided in the challenge.

---

## Step 5: Print a Variable from a Specific Container

If the Pod contains multiple containers:

```bash
kubectl exec <pod-name> -c <container-name> -- printenv <VARIABLE_NAME>
```

---

## Step 6: Verify Pod Details

```bash
kubectl get pod <pod-name> -o wide
```

This displays additional Pod information, including its node and IP address.

---

# Quick Command Summary

```bash
# List Pods
kubectl get pods

# List Pods across all namespaces
kubectl get pods -A

# Inspect the required Pod
kubectl describe pod <pod-name>

# Print the environment variable
kubectl exec <pod-name> -- printenv <VARIABLE_NAME>

# Print a variable from a specific container
kubectl exec <pod-name> -c <container-name> -- printenv <VARIABLE_NAME>

# Display detailed Pod information
kubectl get pod <pod-name> -o wide
```

---

# Understanding the Main Commands

## kubectl get pods

Lists Kubernetes Pods and their statuses.

## kubectl describe pod

Displays detailed information about a specific Pod, including container configuration and environment variables.

## kubectl exec

Executes a command inside a running container.

## printenv

Prints environment variables or the value of a specified variable.

Example:

```bash
printenv APP_COLOR
```

Output:

```text
blue
```

---

# Important Lesson

To print an environment variable in Kubernetes, the most important command is:

```bash
kubectl exec <pod-name> -- printenv <VARIABLE_NAME>
```

Always use the correct Pod name and environment variable name from the challenge.

**Day 57 completed successfully.** 🚀
