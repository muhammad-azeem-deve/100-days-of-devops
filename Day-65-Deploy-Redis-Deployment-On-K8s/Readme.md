# Day 65 - Deploy Redis Deployment on Kubernetes

## Challenge Overview

This is Day 65 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Kubernetes Deployments, ConfigMaps, and Volume Management**. The task required deploying Redis on a Kubernetes cluster using a Deployment with a custom configuration, CPU resource request, and two mounted volumes.

The objective was to create a Redis Deployment using the `redis:alpine` image, configure its memory settings through a ConfigMap, mount the required volumes, expose the Redis container port, and verify that the Deployment was running successfully.

### Challenge Requirement

Create a Redis Deployment on the Kubernetes cluster with the following requirements:

- Create a ConfigMap named `my-redis-config`.
- Configure `maxmemory` to `2mb` in the `redis-config` configuration.
- Create a Deployment named `redis-deployment`.
- Use the `redis:alpine` image.
- Set the container name to `redis-container`.
- Configure only `1` replica.
- Set the CPU request to `1`.
- Mount an EmptyDir volume named `data` at `/redis-master-data`.
- Mount a ConfigMap volume named `redis-config` at `/redis-master`.
- Expose container port `6379`.
- Make sure the Redis Deployment is up and running.

---

## Objectives

The objectives of this challenge are:

- Access the Kubernetes cluster using the jump host.
- Understand how Kubernetes Deployments work.
- Create a ConfigMap for Redis configuration.
- Deploy Redis using the `redis:alpine` image.
- Configure the number of replicas.
- Set CPU resource requests.
- Create and mount an EmptyDir volume.
- Mount a ConfigMap volume inside the Redis container.
- Configure Redis to load its custom configuration.
- Expose the Redis container port.
- Verify the Deployment and Pod status.
- Understand how Kubernetes manages application configuration and storage.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 65 |
| Technology | Kubernetes |
| Application | Redis |
| Image | `redis:alpine` |
| ConfigMap | `my-redis-config` |
| Configuration Key | `redis-config` |
| Memory Configuration | `maxmemory 2mb` |
| Deployment | `redis-deployment` |
| Container | `redis-container` |
| Replicas | `1` |
| CPU Request | `1` |
| EmptyDir Volume | `data` |
| EmptyDir Mount Path | `/redis-master-data` |
| ConfigMap Volume | `redis-config` |
| ConfigMap Mount Path | `/redis-master` |
| Container Port | `6379` |
| Status | Completed |

---

# Solution

## Step 1: Access the Kubernetes Cluster

First, I accessed the Kubernetes environment through the provided jump host.

The `kubectl` utility was already configured to communicate with the Kubernetes cluster.

I verified the cluster nodes using:

```bash
kubectl get nodes
```

This command displays all the nodes available in the Kubernetes cluster.

---

## Step 2: Create the YAML Configuration File

Next, I created a YAML file to define the Redis ConfigMap and Deployment.

```bash
nano redis-deployment.yaml
```

I used a single YAML file containing two Kubernetes resources:

1. ConfigMap
2. Deployment

---

## Step 3: Create the Redis ConfigMap

First, I created a ConfigMap named:

```text
my-redis-config
```

The ConfigMap stores the Redis configuration:

```text
maxmemory 2mb
```

The configuration section was:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-redis-config
data:
  redis-config: |
    maxmemory 2mb
```

A ConfigMap allows us to store application configuration separately from the container image.

This makes it easier to manage application settings without rebuilding the image.

---

## Step 4: Create the Redis Deployment

After defining the ConfigMap, I created the Redis Deployment.

The Deployment configuration included:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
        - name: redis-container
          image: redis:alpine
```

The important configurations were:

- Deployment name: `redis-deployment`
- Container name: `redis-container`
- Image: `redis:alpine`
- Replicas: `1`

A Deployment ensures that Kubernetes maintains the required number of application Pods.

---

## Step 5: Configure Redis Container Port

Next, I configured the Redis container to expose port `6379`.

```yaml
ports:
  - containerPort: 6379
```

Redis uses port `6379` by default for client connections.

---

## Step 6: Configure CPU Resource Request

The task required the Redis container to request one CPU.

I configured:

```yaml
resources:
  requests:
    cpu: "1"
```

This tells Kubernetes that the Redis container requires one CPU core.

The Kubernetes scheduler uses this information when deciding where to place the Pod.

---

## Step 7: Configure Volume Mounts

The task required two volumes.

### Volume 1: EmptyDir

I created an EmptyDir volume named `data` and mounted it at:

```text
/redis-master-data
```

Configuration:

```yaml
volumeMounts:
  - name: data
    mountPath: /redis-master-data
```

Volume definition:

```yaml
volumes:
  - name: data
    emptyDir: {}
```

An EmptyDir volume provides temporary storage that exists for the lifetime of the Pod.

### Volume 2: ConfigMap

I created a ConfigMap volume named `redis-config` and mounted it at:

```text
/redis-master
```

Configuration:

```yaml
volumeMounts:
  - name: redis-config
    mountPath: /redis-master
```

Volume definition:

```yaml
volumes:
  - name: redis-config
    configMap:
      name: my-redis-config
```

This makes the Redis configuration file available inside the container.

---

## Step 8: Configure Redis to Use the ConfigMap

After mounting the ConfigMap, I configured Redis to load its configuration file during startup.

```yaml
args:
  - redis-server
  - /redis-master/redis-config
```

This ensures that Redis reads the configuration containing:

```text
maxmemory 2mb
```

Mounting a ConfigMap makes its files available inside the container, but Redis must also be instructed to read the configuration file.

---

## Step 9: Apply the YAML Configuration

After completing the YAML configuration, I saved the file and applied it to the Kubernetes cluster.

```bash
kubectl apply -f redis-deployment.yaml
```

This command creates the ConfigMap and Redis Deployment.

---

## Step 10: Verify the ConfigMap

After applying the configuration, I checked whether the ConfigMap was created successfully.

```bash
kubectl get configmap my-redis-config
```

To inspect its configuration:

```bash
kubectl describe configmap my-redis-config
```

The ConfigMap should contain:

```text
maxmemory 2mb
```

---

## Step 11: Verify the Deployment

Next, I checked the Redis Deployment:

```bash
kubectl get deployments
```

I also checked its detailed information:

```bash
kubectl describe deployment redis-deployment
```

The Deployment should show one desired replica and one available replica.

---

## Step 12: Verify the Redis Pod

Finally, I checked the Pod status:

```bash
kubectl get pods
```

The Redis Pod should be in:

```text
Running
```

The Ready status should be:

```text
1/1
```

I also verified that Redis was responding:

```bash
kubectl exec deployment/redis-deployment -- redis-cli ping
```

Expected output:

```text
PONG
```

This confirms that Redis is responding to requests inside the container.

---

# Complete YAML Configuration

The complete configuration used for this challenge is:

```yaml
# Redis ConfigMap
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-redis-config
data:
  redis-config: |
    maxmemory 2mb

---
# Redis Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
        - name: redis-container
          image: redis:alpine
          args:
            - redis-server
            - /redis-master/redis-config
          ports:
            - containerPort: 6379
          resources:
            requests:
              cpu: "1"
          volumeMounts:
            - name: data
              mountPath: /redis-master-data
            - name: redis-config
              mountPath: /redis-master

      volumes:
        - name: data
          emptyDir: {}

        - name: redis-config
          configMap:
            name: my-redis-config
```

---

# What I Learned

From this challenge, I learned and practiced:

- How Kubernetes Deployments manage application Pods.
- How to deploy Redis using the `redis:alpine` image.
- How to create and manage ConfigMaps.
- How to store application configuration separately from container images.
- How to configure Redis memory settings using `maxmemory`.
- How to configure container CPU requests.
- How to create EmptyDir volumes.
- How to mount ConfigMaps inside containers.
- How to configure container ports.
- How to use `kubectl apply` to deploy Kubernetes resources.
- How to verify Deployments, ConfigMaps, and Pods.
- How to execute commands inside a running Kubernetes container.
- How Redis can be used as an in-memory caching solution.

# Challenge Status

Day 65 — Completed Successfully ✅

**Redis successfully deployed on Kubernetes with a ConfigMap, CPU resource request, two mounted volumes, and container port 6379.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
