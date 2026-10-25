# Day 58 - Commands

This file contains the commands for **Day 58 of my 100 Days of DevOps Challenge by KodeKloud**.

The objective was to deploy Grafana on a Kubernetes cluster and verify the deployment and Service.

---

## Step 1: Check Kubernetes Nodes

```bash
kubectl get nodes
```

## Step 2: Check Namespaces

```bash
kubectl get namespaces
```

## Step 3: Create the Grafana Deployment

Create the manifest:

```bash
nano grafana-deployment.yaml
```

Apply the Deployment:

```bash
kubectl apply -f grafana-deployment.yaml
```

## Step 4: Verify the Deployment

```bash
kubectl get deployments
```

Get detailed information:

```bash
kubectl describe deployment grafana
```

## Step 5: Check the Pods

```bash
kubectl get pods
```

Get detailed Pod information:

```bash
kubectl describe pod <grafana-pod-name>
```

Check Pod logs:

```bash
kubectl logs <grafana-pod-name>
```

## Step 6: Create the Grafana Service

Create the Service manifest:

```bash
nano grafana-service.yaml
```

Apply it:

```bash
kubectl apply -f grafana-service.yaml
```

## Step 7: Verify the Service

```bash
kubectl get services
```

Get detailed Service information:

```bash
kubectl describe service grafana
```

## Step 8: Check All Resources

```bash
kubectl get all
```

If the resources are deployed in a specific namespace, use:

```bash
kubectl get all -n <namespace>
```

## Step 9: Access Grafana Using Port Forwarding

```bash
kubectl port-forward service/grafana 3000:3000
```

Open the following address in your browser:

```text
http://localhost:3000
```

Keep the port-forwarding command running while accessing Grafana.

## Step 10: Troubleshooting

Check why a Pod is not starting:

```bash
kubectl describe pod <grafana-pod-name>
```

Check Grafana logs:

```bash
kubectl logs <grafana-pod-name>
```

Check recent cluster events:

```bash
kubectl get events --sort-by=.metadata.creationTimestamp
```

Check Deployment status:

```bash
kubectl rollout status deployment/grafana
```

---

# Quick Command Summary

```bash
# Check cluster
kubectl get nodes
kubectl get namespaces

# Deploy Grafana
kubectl apply -f grafana-deployment.yaml

# Create the Service
kubectl apply -f grafana-service.yaml

# Verify Deployment
kubectl get deployments
kubectl describe deployment grafana

# Verify Pods
kubectl get pods

# Verify Service
kubectl get services
kubectl describe service grafana

# Check all resources
kubectl get all

# Check rollout
kubectl rollout status deployment/grafana

# Access Grafana locally
kubectl port-forward service/grafana 3000:3000

# Troubleshoot
kubectl logs <grafana-pod-name>
kubectl describe pod <grafana-pod-name>
kubectl get events --sort-by=.metadata.creationTimestamp
```

---

# Important Lesson

Kubernetes Deployments manage application Pods, while Services provide a stable way to access them.

For Grafana, the container normally listens on port `3000`. However, the correct Deployment name, namespace, image version, and Service configuration must match the requirements of the actual lab.

**Day 58 — Deploy Grafana on Kubernetes Cluster.** 🚀
