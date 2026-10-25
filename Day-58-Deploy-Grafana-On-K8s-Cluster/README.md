# Day 58 - Deploy Grafana on Kubernetes Cluster

## Challenge Overview

This is Day 58 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Kubernetes Application Deployment and Monitoring**. The objective was to deploy Grafana on a Kubernetes cluster and verify that the application was running successfully.

Grafana is an open-source monitoring and data visualization platform that allows users to create dashboards for displaying metrics and other data.

### Challenge Requirement

Deploy Grafana on a Kubernetes cluster and verify that its resources are running correctly.

The deployment process involves creating the required Kubernetes resources, exposing Grafana through a Service when required, and verifying the deployment.

---

## Objectives

The objectives of this challenge are:

- Understand the purpose of Grafana.
- Verify access to the Kubernetes cluster.
- Deploy Grafana as a Kubernetes application.
- Understand Kubernetes Deployments and Pods.
- Expose Grafana through a Kubernetes Service.
- Verify the status of the Deployment and Pod.
- Check the Service configuration.
- Understand how applications are accessed inside and outside a Kubernetes cluster.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 58 |
| Technology | Kubernetes |
| Application | Grafana |
| Container Port | `3000` |
| Deployment | Grafana |
| Service | Kubernetes Service |
| Status | Completed |

---

# Solution

## Step 1: Verify the Kubernetes Cluster

First, I checked the available Kubernetes nodes and namespaces.

```bash
kubectl get nodes
kubectl get namespaces
```

These commands helped me verify that the cluster was accessible.

## Step 2: Create the Grafana Deployment

I created a Kubernetes Deployment to run Grafana.

Example configuration:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      labels:
        app: grafana
    spec:
      containers:
        - name: grafana
          image: grafana/grafana
          ports:
            - containerPort: 3000
```

I saved the configuration as `grafana-deployment.yaml`.

Then I applied it:

```bash
kubectl apply -f grafana-deployment.yaml
```

The Deployment instructs Kubernetes to run one Grafana replica.

## Step 3: Expose Grafana Using a Service

I created a Kubernetes Service to provide network access to Grafana.

Example configuration:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: grafana
spec:
  type: NodePort
  selector:
    app: grafana
  ports:
    - port: 3000
      targetPort: 3000
      nodePort: 30300
```

I saved this configuration as `grafana-service.yaml`.

Then I applied it:

```bash
kubectl apply -f grafana-service.yaml
```

The Service selects the Grafana Pod using its label and forwards traffic to container port `3000`.

*Note: The example uses NodePort `30300`. The actual lab's required Service type and port must take priority.*

## Step 4: Verify the Deployment

I checked the Deployment:

```bash
kubectl get deployments
```

I checked the Pods:

```bash
kubectl get pods
```

I checked the Services:

```bash
kubectl get services
```

These commands helped verify the state of the Grafana resources.

## Step 5: Access Grafana

If NodePort access is configured and allowed, Grafana can be accessed using:

```text
http://<node-ip>:30300
```

Another option is port forwarding:

```bash
kubectl port-forward service/grafana 3000:3000
```

Then open:

```text
http://localhost:3000
```

The browser must be able to reach the machine running the port-forward command.

---

# What I Learned

From this challenge, I learned and practiced:

- What Grafana is and why it is used.
- How to deploy an application on Kubernetes.
- How Kubernetes Deployments manage Pods.
- How container ports work.
- How Kubernetes Services expose applications.
- How to verify Kubernetes resources using `kubectl`.
- How NodePort and port forwarding provide access to applications.
- Why checking Pod and Deployment status is important during troubleshooting.
- How Grafana can be used for monitoring when connected to an appropriate data source.

# Challenge Status

Day 58 — Completed Successfully ✅

**Deploying monitoring tools on Kubernetes is an important step toward understanding application observability.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
