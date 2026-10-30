# Day 61 - Init Container in Kubernetes

## Challenge Overview

This is Day 61 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Kubernetes Init Containers and Shared Volumes**. The task required creating a Deployment with one replica, configuring an Init Container to create a welcome message file, and configuring a Main Container to read that file repeatedly.

To share data between both containers, I used an `emptyDir` volume mounted at `/ic`.

### Challenge Requirement

Create a Kubernetes Deployment named `ic-deploy-datacenter` with the following configuration:

- Create a Deployment with 1 replica.
- Use matching labels `app: ic-datacenter`.
- Configure an Init Container named `ic-msg-datacenter`.
- Use the `ubuntu:latest` image.
- Write a welcome message into `/ic/blog`.
- Configure a Main Container named `ic-main-datacenter`.
- Use the `ubuntu:latest` image.
- Read and display the file contents every 5 seconds.
- Share data using an `emptyDir` volume named `ic-volume-datacenter`.
- Mount the shared volume at `/ic` in both containers.

---

## Objectives

The objectives of this challenge are:

- Understand Kubernetes Init Containers.
- Create a Deployment with one replica.
- Configure an Init Container.
- Configure a Main Container.
- Use the Ubuntu Docker image.
- Create a file inside an Init Container.
- Share files between containers using an `emptyDir` volume.
- Configure volume mounts for multiple containers.
- Use shell commands inside Kubernetes containers.
- Verify container logs using `kubectl logs`.
- Understand the execution order of Init Containers and Main Containers.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 61 |
| Resource | Deployment |
| Deployment Name | `ic-deploy-datacenter` |
| Replicas | 1 |
| Label | `app: ic-datacenter` |
| Init Container | `ic-msg-datacenter` |
| Main Container | `ic-main-datacenter` |
| Image | `ubuntu:latest` |
| Volume Type | `emptyDir` |
| Volume Name | `ic-volume-datacenter` |
| Mount Path | `/ic` |
| File | `/ic/blog` |
| Execution Interval | Every 5 Seconds |
| Status | Completed |

---

# Solution

## Step 1: Understand the Init Container

An Init Container is a special container that runs before the Main Container starts.

It performs initialization tasks such as:

- Creating required files.
- Preparing application data.
- Checking dependencies.
- Performing initial configurations.

In this challenge, the Init Container was responsible for creating a welcome message file.

---

## Step 2: Understand the Shared Volume

Both containers need access to the same file.

For this purpose, I used an `emptyDir` volume.

The volume configuration was:

```yaml
volumes:
  - name: ic-volume-datacenter
    emptyDir: {}
```

I mounted this volume inside both containers at:

```text
/ic
```

This allowed the Init Container to create a file and the Main Container to read the same file.

---

## Step 3: Create the YAML Configuration File

First, I created a YAML file:

```bash
nano init-container.yaml
```

Then I added the following Kubernetes configuration:

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: ic-deploy-datacenter
  labels:
    app: ic-datacenter

spec:
  replicas: 1

  selector:
    matchLabels:
      app: ic-datacenter

  template:
    metadata:
      labels:
        app: ic-datacenter

    spec:
      initContainers:
        - name: ic-msg-datacenter
          image: ubuntu:latest
          command:
            - /bin/bash
            - -c
            - "echo 'Welcome to KodeKloud!' > /ic/blog"
          volumeMounts:
            - name: ic-volume-datacenter
              mountPath: /ic

      containers:
        - name: ic-main-datacenter
          image: ubuntu:latest
          command:
            - /bin/bash
            - -c
            - "while true; do cat /ic/blog; sleep 5; done"
          volumeMounts:
            - name: ic-volume-datacenter
              mountPath: /ic

      volumes:
        - name: ic-volume-datacenter
          emptyDir: {}
```

---

## Step 4: Understand the Init Container Configuration

The Init Container configuration was:

```yaml
initContainers:
  - name: ic-msg-datacenter
    image: ubuntu:latest
    command:
      - /bin/bash
      - -c
      - "echo 'Welcome to KodeKloud!' > /ic/blog"
```

Here:

- `initContainers` defines the initialization containers.
- `name` specifies the Init Container name.
- `image` specifies the Ubuntu image.
- `/bin/bash` executes shell commands.
- `-c` tells Bash to execute the supplied command.
- `echo` writes the welcome message.
- `>` redirects the message into the `/ic/blog` file.

The Init Container completes after successfully creating the file.

---

## Step 5: Understand the Main Container Configuration

The Main Container configuration was:

```yaml
containers:
  - name: ic-main-datacenter
    image: ubuntu:latest
    command:
      - /bin/bash
      - -c
      - "while true; do cat /ic/blog; sleep 5; done"
```

The command:

```bash
while true; do cat /ic/blog; sleep 5; done
```

works as follows:

1. `while true` creates an infinite loop.
2. `cat /ic/blog` reads the welcome message.
3. `sleep 5` waits for five seconds.
4. The loop repeats continuously.

This keeps the Main Container running and displaying the file contents every 5 seconds.

---

## Step 6: Configure the Shared Volume

The shared volume configuration was:

```yaml
volumes:
  - name: ic-volume-datacenter
    emptyDir: {}
```

Both containers mounted the volume using:

```yaml
volumeMounts:
  - name: ic-volume-datacenter
    mountPath: /ic
```

This allowed both containers to access the same `/ic/blog` file.

---

## Step 7: Apply the Deployment

After completing the YAML configuration, I applied it using:

```bash
kubectl apply -f init-container.yaml
```

This created the Deployment in Kubernetes.

---

## Step 8: Verify the Deployment

I checked whether the Deployment was created successfully:

```bash
kubectl get deployments
```

Expected result:

```text
NAME                   READY   UP-TO-DATE   AVAILABLE
ic-deploy-datacenter   1/1     1            1
```

This confirmed that the Deployment had one available replica.

---

## Step 9: Verify the Pod

Next, I checked the Pod status:

```bash
kubectl get pods
```

I also used the label selector:

```bash
kubectl get pods -l app=ic-datacenter
```

The Pod should eventually reach the `Running` state after the Init Container completes successfully.

---

## Step 10: Verify Init Container Logs

I checked the Init Container logs using:

```bash
kubectl logs <pod-name> -c ic-msg-datacenter
```

The Init Container writes the message into a file instead of printing it to the terminal, so no output is expected.

This is normal.

---

## Step 11: Verify Main Container Logs

Next, I checked the Main Container logs:

```bash
kubectl logs <pod-name> -c ic-main-datacenter
```

Expected output:

```text
Welcome to KodeKloud!
Welcome to KodeKloud!
Welcome to KodeKloud!
Welcome to KodeKloud!
```

The message appeared repeatedly because the Main Container reads the file every 5 seconds.

---

## Step 12: Verify the Shared File

Finally, I verified that the file was accessible inside the Main Container:

```bash
kubectl exec <pod-name> -c ic-main-datacenter -- cat /ic/blog
```

Expected output:

```text
Welcome to KodeKloud!
```

This confirmed that the shared volume was working correctly.

---

# What I Learned

From this challenge, I learned and practiced:

- What Kubernetes Init Containers are.
- How Init Containers execute before Main Containers.
- How to create a Kubernetes Deployment using YAML.
- How to configure one replica.
- How to use matching labels and selectors.
- How to configure Init Containers using `initContainers`.
- How to configure normal application containers.
- How to execute shell commands inside Ubuntu containers.
- How to create files using the `echo` command.
- How to share data between containers using `emptyDir`.
- How to configure `volumeMounts`.
- How to use an infinite loop with `while true`.
- How to use `sleep` to introduce a 5-second interval.
- How to verify container logs using `kubectl logs`.
- How to execute commands inside containers using `kubectl exec`.

# Challenge Status

Day 61 — Completed Successfully ✅

**Learned how Init Containers work, created a shared volume, and successfully configured communication between an Init Container and a Main Container.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
