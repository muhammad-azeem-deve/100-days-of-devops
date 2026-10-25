# Day 56 - Commands

This file contains the commands used during **Day 56 of my 100 Days of DevOps Challenge by KodeKloud**.

The objective was to deploy Nginx with three replicas and expose it through a NodePort Service on port `30011`.

---

# Step 1: Create the Deployment Manifest

Create the Deployment file:

```bash
nano nginx-deployment.yaml
```

Add the following configuration:

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

Save and exit the editor.

---

# Step 2: Apply the Deployment

```bash
kubectl apply -f nginx-deployment.yaml
```

---

# Step 3: Verify the Deployment

```bash
kubectl get deployments
```

Get detailed information:

```bash
kubectl describe deployment nginx-deployment
```

---

# Step 4: Verify the Pods

```bash
kubectl get pods
```

Check the Pods with their labels:

```bash
kubectl get pods --show-labels
```

Wait until all three Pods are running and ready.

---

# Step 5: Verify the ReplicaSet

```bash
kubectl get replicasets
```

Get detailed ReplicaSet information:

```bash
kubectl describe replicaset
```

Kubernetes automatically creates the ReplicaSet for the Deployment.

---

# Step 6: Create the NodePort Service

Create the Service manifest:

```bash
nano nginx-service.yaml
```

Add the following configuration:

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

Save and exit the editor.

---

# Step 7: Apply the Service

```bash
kubectl apply -f nginx-service.yaml
```

---

# Step 8: Verify the Service

```bash
kubectl get services
```

Check the specific Service:

```bash
kubectl get service nginx-service
```

Get detailed information:

```bash
kubectl describe service nginx-service
```

Expected port mapping:

```text
80:30011/TCP
```

---

# Step 9: Verify Service Endpoints

Check the endpoints associated with the Service:

```bash
kubectl get endpoints nginx-service
```

You should see the IP addresses of the ready backend Pods.

On clusters using EndpointSlices, you can also run:

```bash
kubectl get endpointslices
```

---

# Step 10: Test Nginx

If the node IP is reachable from your machine, open:

```text
http://<node-ip>:30011
```

Alternatively, test from a terminal that has network access to the node:

```bash
curl http://<node-ip>:30011
```

The expected response is the default Nginx welcome page HTML.

---

# Troubleshooting Commands

If the Pods are not running:

```bash
kubectl get pods
kubectl describe pods
kubectl get events --sort-by=.metadata.creationTimestamp
```

Check the Nginx logs:

```bash
kubectl logs deployment/nginx-deployment
```

Check the Service configuration:

```bash
kubectl describe service nginx-service
```

Check the Deployment:

```bash
kubectl describe deployment nginx-deployment
```

If the NodePort cannot be accessed, verify that the node is reachable and that firewalls or network rules allow traffic to port `30011`.

---

# Quick Command Summary

```bash
# Apply Deployment
kubectl apply -f nginx-deployment.yaml

# Verify Deployment
kubectl get deployments

# Verify Pods
kubectl get pods

# Verify ReplicaSets
kubectl get replicasets

# Apply NodePort Service
kubectl apply -f nginx-service.yaml

# Verify Services
kubectl get services

# Inspect Service
kubectl describe service nginx-service

# Verify endpoints
kubectl get endpoints nginx-service

# Check application logs
kubectl logs deployment/nginx-deployment

# Test application (replace with reachable node IP)
curl http://<node-ip>:30011
```

---

# Important Kubernetes Concepts

| Component | Purpose |
|-----------|---------|
| Deployment | Manages the desired number of application replicas |
| ReplicaSet | Maintains the required number of matching Pods |
| Pod | Runs the Nginx container |
| Service | Provides a stable endpoint to access the Pods |
| NodePort | Exposes the Service on a port on each node |
| Port `30011` | Required external NodePort |
| Port `80` | Nginx container and Service target port |

---

# Important Lesson

For this challenge, the Deployment was configured with:

```yaml
replicas: 3
```

The Service was configured with:

```yaml
type: NodePort
```

and:

```yaml
nodePort: 30011
```

The Service selector and Pod labels must match:

```yaml
app: nginx
```

This allows Kubernetes to route traffic from the NodePort Service to the Nginx Pods.

**Day 56 completed successfully.** 🚀
