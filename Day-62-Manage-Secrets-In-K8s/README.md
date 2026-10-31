# Day 62 - Kubernetes Secrets

## Challenge Overview

This is Day 62 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Kubernetes Secrets and Secret Management**. The objective was to securely store license information using a Kubernetes Secret and make that information available inside a Pod through a mounted volume.

The task required creating a generic Secret named `beta` from an existing license file, deploying a Pod with a Debian container, and mounting the Secret inside the container at the specified location.

### Challenge Requirement

The objective was to complete the following tasks:

- Create a generic Kubernetes Secret named `beta`.
- Use the existing license file `/opt/beta.txt`.
- Create a Pod named `secret-nautilus`.
- Create a container named `secret-container-nautilus`.
- Use the `debian:latest` image.
- Keep the container running using a `sleep` command.
- Mount the `beta` Secret inside the container at `/opt/games`.

---

## Objectives

The objectives of this challenge are:

- Understand Kubernetes Secrets.
- Create a generic Secret using an existing file.
- Use `kubectl create secret generic`.
- Create a Kubernetes Pod using YAML configuration.
- Configure container names and images.
- Keep a container running using the `sleep` command.
- Mount a Kubernetes Secret as a volume.
- Verify the Secret and Pod.
- Access the mounted Secret file inside the container.
- Understand the relationship between Secrets, volumes, and volume mounts.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 62 |
| Kubernetes Resource | Secret |
| Secret Type | Generic |
| Secret Name | `beta` |
| Source File | `/opt/beta.txt` |
| Pod Name | `secret-nautilus` |
| Container Name | `secret-container-nautilus` |
| Container Image | `debian:latest` |
| Container Command | `sleep 3600` |
| Mount Path | `/opt/games` |
| Status | Completed |

---

# Solution

## Step 1: Check the Existing License File

First, I checked whether the required license file existed.

The file was located at:

```bash
ls -l /opt/beta.txt
```

The file already existed, so I used it to create the Kubernetes Secret.

---

## Step 2: Create the Generic Kubernetes Secret

Next, I created a generic Secret named `beta` using the existing license file.

I used:

```bash
kubectl create secret generic beta --from-file=/opt/beta.txt
```

### Explanation

- `kubectl` is the Kubernetes command-line tool.
- `create secret` creates a Kubernetes Secret.
- `generic` specifies the Secret type.
- `beta` is the Secret name.
- `--from-file` takes data from an existing file.
- `/opt/beta.txt` is the source license file.

Kubernetes uses the filename `beta.txt` as the Secret data key.

---

## Step 3: Verify the Secret

After creating the Secret, I verified that it was successfully created.

```bash
kubectl get secrets
```

I also checked the specific Secret:

```bash
kubectl describe secret beta
```

This confirmed that the Secret existed and contained the expected data key.

---

## Step 4: Create the Pod YAML File

Next, I created a YAML configuration file for the Pod.

```bash
vi secret-pod.yaml
```

I added the following configuration:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-nautilus
spec:
  containers:
    - name: secret-container-nautilus
      image: debian:latest
      command: ["sleep", "3600"]
      volumeMounts:
        - name: beta-volume
          mountPath: /opt/games
          readOnly: true
  volumes:
    - name: beta-volume
      secret:
        secretName: beta
```

### Explanation of YAML Configuration

**Pod Configuration**

```yaml
metadata:
  name: secret-nautilus
```

Defines the Pod name.

**Container Configuration**

```yaml
containers:
  - name: secret-container-nautilus
    image: debian:latest
```

Defines the container name and Docker image.

**Sleep Command**

```yaml
command: ["sleep", "3600"]
```

Keeps the container running for 3600 seconds (one hour).

**Volume Mount**

```yaml
volumeMounts:
  - name: beta-volume
    mountPath: /opt/games
    readOnly: true
```

Mounts the Secret volume inside the container at `/opt/games` and makes it read-only.

**Secret Volume**

```yaml
volumes:
  - name: beta-volume
    secret:
      secretName: beta
```

Connects the Kubernetes Secret named `beta` to the Pod.

---

## Step 5: Deploy the Pod

After creating the YAML configuration, I deployed the Pod using:

```bash
kubectl apply -f secret-pod.yaml
```

The command created the Pod using the specified configuration.

---

## Step 6: Verify the Pod

After deploying the Pod, I checked its status:

```bash
kubectl get pods
```

The expected status was:

```text
NAME              READY   STATUS    RESTARTS
secret-nautilus   1/1     Running   0
```

This confirmed that the Debian container was running successfully.

---

## Step 7: Verify the Secret Mount

Next, I checked whether the license file was mounted inside the container.

I used:

```bash
kubectl exec secret-nautilus -- ls -l /opt/games
```

Since the Secret was created from `/opt/beta.txt`, the mounted file was available as:

```text
/opt/games/beta.txt
```

I also verified the specific file:

```bash
kubectl exec secret-nautilus -- ls -l /opt/games/beta.txt
```

This confirmed that the Secret was successfully mounted inside the container.

---

## Step 8: Verify the Final Configuration

Finally, I performed the following checks:

```bash
kubectl get secret beta
```

```bash
kubectl get pod secret-nautilus
```

```bash
kubectl describe pod secret-nautilus
```

```bash
kubectl exec secret-nautilus -- ls /opt/games
```

These commands helped verify that:

- The Secret `beta` existed.
- The Pod `secret-nautilus` was running.
- The correct container was deployed.
- The Secret volume was mounted at `/opt/games`.
- The license file was accessible inside the container.

---

# What I Learned

From this challenge, I learned and practiced:

- What Kubernetes Secrets are and why they are used.
- How to create a generic Secret using an existing file.
- How to use `kubectl create secret generic`.
- How to create a Pod using a YAML configuration file.
- How to configure a Debian container.
- How to keep a container running using the `sleep` command.
- How Kubernetes volumes work.
- How to mount a Secret inside a container.
- The difference between `volumes` and `volumeMounts`.
- How to verify Kubernetes Secrets and Pods.
- How to access files inside containers using `kubectl exec`.
- Why sensitive information should not be directly hardcoded into Pod configurations.

# Challenge Status

Day 62 — Completed Successfully ✅

**Kubernetes Secret created successfully, and license information mounted inside the Debian container.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
