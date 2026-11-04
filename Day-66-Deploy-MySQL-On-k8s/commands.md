# Day 66 - Commands

This file contains the commands used during **Day 66 of my 100 Days of DevOps Challenge by KodeKloud**.

The objective was to deploy MySQL on Kubernetes using PersistentVolume, PersistentVolumeClaim, Secrets, Deployment, and NodePort Service.

---

# Step 1: Check Kubernetes Nodes

```bash
kubectl get nodes
```

---

# Step 2: Create Persistent Storage Directory

Create the directory on the Kubernetes node providing hostPath storage:

```bash
sudo mkdir -p /mnt/data/mysql
```

---

# Step 3: Create PersistentVolume

Create the YAML file:

```bash
vi mysql-pv.yaml
```

Configuration:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 250Mi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: ""
  hostPath:
    path: /mnt/data/mysql
```

Apply:

```bash
kubectl apply -f mysql-pv.yaml
```

---

# Step 4: Create PersistentVolumeClaim

Create the file:

```bash
vi mysql-pvc.yaml
```

Configuration:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: ""
  resources:
    requests:
      storage: 250Mi
```

Apply:

```bash
kubectl apply -f mysql-pvc.yaml
```

Verify:

```bash
kubectl get pv
kubectl get pvc
```

---

# Step 5: Create Kubernetes Secrets

### Create Root Password Secret

```bash
kubectl create secret generic mysql-root-pass \
  --from-literal=password='<ROOT_PASSWORD>'
```

### Create MySQL User Secret

```bash
kubectl create secret generic mysql-user-pass \
  --from-literal=username='<MYSQL_USERNAME>' \
  --from-literal=password='<MYSQL_USER_PASSWORD>'
```

### Create Database Secret

```bash
kubectl create secret generic mysql-db-url \
  --from-literal=database='<DATABASE_NAME>'
```

Replace the placeholders with the exact values provided in the KodeKloud task.

Verify:

```bash
kubectl get secrets
```

---

# Step 6: Create MySQL Deployment

Create the file:

```bash
vi mysql-deployment.yaml
```

Configuration:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql:5.7
          ports:
            - containerPort: 3306
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-root-pass
                  key: password
            - name: MYSQL_DATABASE
              valueFrom:
                secretKeyRef:
                  name: mysql-db-url
                  key: database
            - name: MYSQL_USER
              valueFrom:
                secretKeyRef:
                  name: mysql-user-pass
                  key: username
            - name: MYSQL_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-user-pass
                  key: password
          volumeMounts:
            - name: mysql-storage
              mountPath: /var/lib/mysql
      volumes:
        - name: mysql-storage
          persistentVolumeClaim:
            claimName: mysql-pv-claim
```

Apply:

```bash
kubectl apply -f mysql-deployment.yaml
```

---

# Step 7: Create MySQL NodePort Service

Create the file:

```bash
vi mysql-service.yaml
```

Configuration:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  type: NodePort
  selector:
    app: mysql
  ports:
    - port: 3306
      targetPort: 3306
      nodePort: 30007
```

Apply:

```bash
kubectl apply -f mysql-service.yaml
```

---

# Step 8: Verify PersistentVolume and PVC

```bash
kubectl get pv
kubectl get pvc
```

Check detailed information:

```bash
kubectl describe pv mysql-pv
kubectl describe pvc mysql-pv-claim
```

---

# Step 9: Verify Secrets

```bash
kubectl get secrets
```

Check individual Secrets:

```bash
kubectl describe secret mysql-root-pass
kubectl describe secret mysql-user-pass
kubectl describe secret mysql-db-url
```

---

# Step 10: Verify Deployment

```bash
kubectl get deployments
```

Detailed information:

```bash
kubectl describe deployment mysql-deployment
```

---

# Step 11: Verify Pods

```bash
kubectl get pods
```

Check detailed Pod information:

```bash
kubectl describe pod <mysql-pod-name>
```

Check container logs:

```bash
kubectl logs <mysql-pod-name>
```

---

# Step 12: Verify MySQL Service

```bash
kubectl get services
```

Detailed information:

```bash
kubectl describe service mysql
```

---

# Step 13: Check All Resources Together

```bash
kubectl get all
```

Check storage resources:

```bash
kubectl get pv,pvc
```

Check Secrets:

```bash
kubectl get secrets
```

---

# Quick Command Summary

```bash
# Check Kubernetes nodes
kubectl get nodes

# Create hostPath directory on storage node
sudo mkdir -p /mnt/data/mysql

# Create PersistentVolume
kubectl apply -f mysql-pv.yaml

# Create PersistentVolumeClaim
kubectl apply -f mysql-pvc.yaml

# Verify storage
kubectl get pv
kubectl get pvc

# Create MySQL Secrets
kubectl create secret generic mysql-root-pass \
  --from-literal=password='<ROOT_PASSWORD>'

kubectl create secret generic mysql-user-pass \
  --from-literal=username='<MYSQL_USERNAME>' \
  --from-literal=password='<MYSQL_USER_PASSWORD>'

kubectl create secret generic mysql-db-url \
  --from-literal=database='<DATABASE_NAME>'

# Verify Secrets
kubectl get secrets

# Deploy MySQL
kubectl apply -f mysql-deployment.yaml

# Create MySQL Service
kubectl apply -f mysql-service.yaml

# Verify Deployment
kubectl get deployments

# Verify Pods
kubectl get pods

# Verify Service
kubectl get services

# Check all resources
kubectl get all
```

---

# Troubleshooting Commands

### Check Pod Status

```bash
kubectl get pods
```

### Describe Pod

```bash
kubectl describe pod <mysql-pod-name>
```

### Check MySQL Logs

```bash
kubectl logs <mysql-pod-name>
```

### Check PersistentVolume

```bash
kubectl describe pv mysql-pv
```

### Check PersistentVolumeClaim

```bash
kubectl describe pvc mysql-pv-claim
```

### Check Deployment

```bash
kubectl describe deployment mysql-deployment
```

### Check Service

```bash
kubectl describe service mysql
```

---

# Important Kubernetes Concepts

| Command / Resource | Purpose |
|---|---|
| `kubectl apply` | Creates or updates resources |
| `kubectl get` | Displays resources |
| `kubectl describe` | Shows detailed resource information |
| `kubectl logs` | Displays container logs |
| PV | Provides persistent storage |
| PVC | Requests persistent storage |
| Secret | Stores sensitive configuration values |
| Deployment | Manages application Pods |
| NodePort | Exposes a Service through a node port |

---

# Important Lesson

For deploying a database on Kubernetes, persistent storage is important because database data should not disappear when a Pod is deleted or recreated.

Kubernetes Secrets help separate credentials from the Deployment configuration.

The most important configurations in this challenge were:

```text
PersistentVolume: mysql-pv
PersistentVolumeClaim: mysql-pv-claim
Deployment: mysql-deployment
Service: mysql
NodePort: 30007
Mount Path: /var/lib/mysql
```

The three Secrets were:

```text
mysql-root-pass
mysql-user-pass
mysql-db-url
```

**Day 66 completed successfully.** 🚀
