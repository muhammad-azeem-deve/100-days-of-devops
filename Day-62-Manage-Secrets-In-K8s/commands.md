# Day 62 - Commands

This file contains the commands used during **Day 62 of my 100 Days of DevOps Challenge by KodeKloud**.

The objective was to create a generic Kubernetes Secret named `beta` using `/opt/beta.txt` and mount it inside a Debian container at `/opt/games`.

---

# Step 1: Check the License File

Check whether the existing license file is available:

```bash
ls -l /opt/beta.txt
```

---

# Step 2: Create the Generic Secret

Create a Kubernetes Secret named `beta`:

```bash
kubectl create secret generic beta --from-file=/opt/beta.txt
```

This creates a generic Secret using the contents of the existing license file.

---

# Step 3: Verify the Secret

List all Kubernetes Secrets:

```bash
kubectl get secrets
```

Check the specific Secret:

```bash
kubectl describe secret beta
```

---

# Step 4: Create the Pod YAML File

Create the YAML file:

```bash
vi secret-pod.yaml
```

Add the following configuration:

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

Save and exit the file.

---

# Step 5: Deploy the Pod

Apply the YAML configuration:

```bash
kubectl apply -f secret-pod.yaml
```

---

# Step 6: Verify the Pod

Check all Pods:

```bash
kubectl get pods
```

Check the specific Pod:

```bash
kubectl get pod secret-nautilus
```

Get detailed information:

```bash
kubectl describe pod secret-nautilus
```

---

# Step 7: Verify the Mounted Secret

List the files inside the mounted directory:

```bash
kubectl exec secret-nautilus -- ls -l /opt/games
```

Check the specific license file:

```bash
kubectl exec secret-nautilus -- ls -l /opt/games/beta.txt
```

View the license file contents if needed:

```bash
kubectl exec secret-nautilus -- cat /opt/games/beta.txt
```

---

# Step 8: Verify the Container

Check the running container:

```bash
kubectl get pod secret-nautilus
```

Execute a command inside the container:

```bash
kubectl exec secret-nautilus -- hostname
```

---

# Quick Command Summary

```bash
# Check the existing license file
ls -l /opt/beta.txt

# Create generic Kubernetes Secret
kubectl create secret generic beta --from-file=/opt/beta.txt

# Verify Secret
kubectl get secrets
kubectl describe secret beta

# Create Pod YAML file
vi secret-pod.yaml

# Deploy Pod
kubectl apply -f secret-pod.yaml

# Verify Pod
kubectl get pods
kubectl describe pod secret-nautilus

# Verify mounted Secret
kubectl exec secret-nautilus -- ls -l /opt/games

# Verify license file
kubectl exec secret-nautilus -- ls -l /opt/games/beta.txt
```

---

# Understanding Kubernetes Secret Commands

## Create Secret

```bash
kubectl create secret generic beta --from-file=/opt/beta.txt
```

Creates a generic Secret named `beta` using the existing license file.

## Get Secret

```bash
kubectl get secrets
```

Displays the available Secrets in the current namespace.

## Describe Secret

```bash
kubectl describe secret beta
```

Displays information about the Secret without printing its actual data values.

## Apply Pod Configuration

```bash
kubectl apply -f secret-pod.yaml
```

Creates the Pod using the YAML configuration.

## Execute Commands Inside a Pod

```bash
kubectl exec secret-nautilus -- ls /opt/games
```

Executes the `ls` command inside the running container.

---

# Important Kubernetes YAML Configuration

The most important part of this challenge was connecting the Secret to the Pod.

**Secret Volume:**

```yaml
volumes:
  - name: beta-volume
    secret:
      secretName: beta
```

**Volume Mount:**

```yaml
volumeMounts:
  - name: beta-volume
    mountPath: /opt/games
    readOnly: true
```

Both use the same volume name:

```text
beta-volume
```

The Secret name is:

```text
beta
```

The mount location is:

```text
/opt/games
```

The mounted license file is:

```text
/opt/games/beta.txt
```

---

# Important Lesson

A Kubernetes Secret can be created from an existing file and mounted inside a container as a volume.

For this challenge, the two most important configurations were:

**1. Creating the Secret:**

```bash
kubectl create secret generic beta --from-file=/opt/beta.txt
```

**2. Mounting the Secret inside the Pod:**

```yaml
volumes:
  - name: beta-volume
    secret:
      secretName: beta
```

The `volumeMounts` configuration determines where the Secret becomes accessible inside the container.

Always verify both the Pod status and the mounted file after deploying the configuration.

**Day 62 completed successfully.** 🚀
