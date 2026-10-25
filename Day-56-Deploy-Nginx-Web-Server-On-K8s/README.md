# Day 56 - Deploy Nginx Web Server on Kubernetes Cluster

## Challenge Overview

This is Day 56 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Kubernetes Deployments and Services**. The objective was to deploy an Nginx web server on a Kubernetes cluster with three replicas and expose the application using a NodePort service on port `30011`.

To complete the task, I created a Kubernetes Deployment, configured three replicas, created a NodePort Service, and verified that the Pods and Service were configured correctly.

### Challenge Requirement

Deploy an Nginx web server on the Kubernetes cluster with:

- Three replicas.
- An Nginx container image.
- A NodePort Service.
- NodePort `30011`.

The application needed to be accessible through the NodePort service.

---

## Objectives

The objectives of this challenge are:

- Understand Kubernetes Deployments.
- Deploy Nginx using a Kubernetes Deployment.
- Configure three replicas.
- Understand how Kubernetes manages ReplicaSets and Pods.
- Create a NodePort Service.
- Expose the application on port `30011`.
- Verify the Deployment, Pods, ReplicaSet, and Service.
- Understand how Kubernetes Services route traffic to Pods.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 56 |
| Technology | Kubernetes |
| Application | Nginx |
| Container Image | `nginx:latest` |
| Resource | Deployment |
| Replicas | 3 |
| Service Type | NodePort |
| NodePort | `30011` |
| Container Port | `80` |
| Status | Completed |

---

# Solution

## Step 1: Create the Nginx Deployment

First, I created a Deployment manifest named `nginx-deployment.yaml`.

The Deployment manages the Nginx application and maintains three replicas.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
```

### Explanation

- `apiVersion: apps/v1` specifies the Kubernetes API version.
- `kind: Deployment` defines the resource as a Deployment.
- `metadata.name` specifies the Deployment name.
- `replicas: 3` instructs Kubernetes to maintain three Pods.
- `selector.matchLabels` identifies the Pods managed by the Deployment.
- `template` defines the Pod configuration.
- `image: nginx:latest` specifies the Nginx container image.
- `containerPort: 80` documents the port used by Nginx inside the container.

---

## Step 2: Apply the Deployment

After creating the Deployment manifest, I applied it to the Kubernetes cluster.

```bash
kubectl apply -f nginx-deployment.yaml
```

This command creates the Deployment and allows Kubernetes to manage the three Nginx Pods.

---

## Step 3: Verify the Deployment and Pods

I verified the Deployment using:

```bash
kubectl get deployments
```

Then I checked the Pods:

```bash
kubectl get pods
```

The expected result was three Nginx Pods in the Running state once the deployment completed successfully.

I also checked the ReplicaSets:

```bash
kubectl get replicasets
```

Kubernetes automatically creates and manages a ReplicaSet for the Deployment.

---

## Step 4: Create the NodePort Service

Next, I created a Service manifest named `nginx-service.yaml`.

The Service exposes Nginx through NodePort `30011`.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30011
```

### Explanation

- `kind: Service` defines a Kubernetes Service.
- `type: NodePort` exposes the Service through a port on the Kubernetes nodes.
- `selector.app: nginx` selects the Pods labelled `app: nginx`.
- `port: 80` is the port exposed by the Service.
- `targetPort: 80` forwards traffic to port 80 on the selected Pods.
- `nodePort: 30011` specifies the external node port required by the challenge.

---

## Step 5: Apply the Service

I applied the Service manifest:

```bash
kubectl apply -f nginx-service.yaml
```

This created the NodePort Service and connected it to the matching Nginx Pods.

---

## Step 6: Verify the Service

I checked the Service configuration using:

```bash
kubectl get services
```

For detailed information about the Nginx Service, I used:

```bash
kubectl describe service nginx-service
```

The Service should show NodePort `30011` and target port `80`.

The port mapping should look similar to:

```text
80:30011/TCP
```

---

## Step 7: Test the Nginx Application

If the Kubernetes node is reachable and network access is allowed, the application can be accessed using:

```text
http://<node-ip>:30011
```

The expected result is the default Nginx welcome page.

If the application is not accessible, I can investigate the Pod status, Service endpoints, and network configuration.

---

# Kubernetes Architecture

The deployment works as follows:

```text
User / Browser
      |
      | Port 30011
      v
NodePort Service
      |
      | Service Port 80
      v
Nginx Pods
      |
      +---- Pod 1
      |
      +---- Pod 2
      |
      +---- Pod 3
```

The Deployment maintains the desired number of Pods, while the Service provides a stable way to reach the application.

---

# What I Learned

From this challenge, I learned and practiced:

- How to deploy an application on Kubernetes.
- How to create a Deployment using YAML.
- How to configure three replicas.
- How Kubernetes automatically manages ReplicaSets.
- How to create a NodePort Service.
- How to expose an application through port `30011`.
- How labels and selectors connect Services to Pods.
- How to use `kubectl apply` to deploy resources.
- How to verify Deployments, Pods, ReplicaSets, and Services.
- How to troubleshoot basic application connectivity issues in Kubernetes.

# Challenge Status

Day 56 — Completed Successfully ✅

**Nginx deployed with three replicas and exposed through a NodePort Service on port 30011.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
