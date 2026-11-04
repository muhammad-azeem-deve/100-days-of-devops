# Day 66 - Deploy MySQL on Kubernetes

## Challenge Overview

This is Day 66 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Kubernetes Deployment, Persistent Storage, and Secrets Management**. The objective was to deploy a MySQL database inside a Kubernetes cluster, configure persistent storage, securely manage database credentials using Kubernetes Secrets, and expose MySQL through a NodePort service.

### Challenge Requirement

Deploy a MySQL server on the Kubernetes cluster with the following requirements:

1. Create a PersistentVolume named `mysql-pv` with a capacity of `250Mi`.
2. Create a PersistentVolumeClaim named `mysql-pv-claim` requesting `250Mi` storage.
3. Create a Deployment named `mysql-deployment` using a MySQL image.
4. Mount persistent storage at `/var/lib/mysql`.
5. Create a NodePort Service named `mysql` with NodePort `30007`.
6. Create three Kubernetes Secrets for MySQL credentials and database configuration.
7. Configure the MySQL container environment variables using `secretKeyRef`.

---

## Objectives

The objectives of this challenge are:

- Understand how to deploy MySQL on Kubernetes.
- Create a PersistentVolume (PV).
- Create a PersistentVolumeClaim (PVC).
- Connect persistent storage to a MySQL Pod.
- Create Kubernetes Secrets.
- Store MySQL passwords and database credentials securely.
- Configure environment variables using `secretKeyRef`.
- Create a Kubernetes Deployment.
- Expose MySQL using a NodePort Service.
- Verify Kubernetes resources using `kubectl`.
- Understand how persistent storage protects database data.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 66 |
| Technology | Kubernetes |
| Application | MySQL |
| MySQL Image | `mysql:5.7` |
| PersistentVolume | `mysql-pv` |
| PV Capacity | `250Mi` |
| PersistentVolumeClaim | `mysql-pv-claim` |
| PVC Request | `250Mi` |
| Deployment | `mysql-deployment` |
| Service | `mysql` |
| Service Type | `NodePort` |
| NodePort | `30007` |
| Secrets | 3 |
| Mount Path | `/var/lib/mysql` |
| Status | Completed |

---

# Solution

## Step 1: Check Kubernetes Cluster

First, I accessed the jump host where `kubectl` was already configured to communicate with the Kubernetes cluster.

I checked the available nodes:

```bash
kubectl get nodes
```

This helped me verify that the Kubernetes cluster was accessible.

---

## Step 2: Create Persistent Storage Directory

Before creating the PersistentVolume, I prepared the directory used for storing MySQL data on the Kubernetes node.

Example:

```bash
sudo mkdir -p /mnt/data/mysql
```

This directory was used as the hostPath storage location.

**Note:** The directory must exist on the Kubernetes node providing the storage, not just on the jump host.

---

## Step 3: Create PersistentVolume

Next, I created a PersistentVolume named `mysql-pv`.

I created the YAML file:

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

Apply the configuration:

```bash
kubectl apply -f mysql-pv.yaml
```

The PersistentVolume provides `250Mi` of storage for the MySQL database.

---

## Step 4: Create PersistentVolumeClaim

After creating the PV, I created a PersistentVolumeClaim named `mysql-pv-claim`.

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

Apply the configuration:

```bash
kubectl apply -f mysql-pvc.yaml
```

The PVC requests `250Mi` of storage from the PersistentVolume.

I verified the storage resources:

```bash
kubectl get pv
kubectl get pvc
```

The expected status was:

```text
STATUS: Bound
```

This indicates that the PVC successfully claimed the PersistentVolume.

---

## Step 5: Create Kubernetes Secrets

The task required creating three Secrets to store MySQL credentials and database information.

Instead of writing passwords directly inside the Deployment, I used Kubernetes Secrets.

### Secret 1: mysql-root-pass

This Secret stores the MySQL root password.

```bash
kubectl create secret generic mysql-root-pass \
  --from-literal=password='<ROOT_PASSWORD>'
```

### Secret 2: mysql-user-pass

This Secret stores the MySQL username and password.

```bash
kubectl create secret generic mysql-user-pass \
  --from-literal=username='<MYSQL_USERNAME>' \
  --from-literal=password='<MYSQL_USER_PASSWORD>'
```

### Secret 3: mysql-db-url

This Secret stores the database name.

```bash
kubectl create secret generic mysql-db-url \
  --from-literal=database='<DATABASE_NAME>'
```

**Important:** I used the exact username, passwords, and database name provided in my KodeKloud challenge.

The required Secret names and keys were:

| Secret Name | Key |
|---|---|
| `mysql-root-pass` | `password` |
| `mysql-user-pass` | `username` |
| `mysql-user-pass` | `password` |
| `mysql-db-url` | `database` |

---

## Step 6: Create MySQL Deployment

Next, I created a Deployment named `mysql-deployment`.

The Deployment uses the MySQL image and connects the PersistentVolumeClaim to the MySQL container.

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

Apply the Deployment:

```bash
kubectl apply -f mysql-deployment.yaml
```

### Understanding the Deployment

The important configurations are:

- `replicas: 1` creates one MySQL Pod.
- `image: mysql:5.7` uses the MySQL container image.
- `containerPort: 3306` specifies the MySQL port.
- `secretKeyRef` retrieves credentials from Kubernetes Secrets.
- `volumeMounts` connects persistent storage to the container.
- `claimName` connects the Deployment to `mysql-pv-claim`.

The MySQL database files are stored at:

```text
/var/lib/mysql
```

---

## Step 7: Create MySQL NodePort Service

After creating the Deployment, I exposed MySQL using a NodePort Service.

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

Apply the Service:

```bash
kubectl apply -f mysql-service.yaml
```

### Understanding the Service

| Configuration | Purpose |
|---|---|
| `type: NodePort` | Exposes the service through a node port |
| `port: 3306` | Service port |
| `targetPort: 3306` | MySQL container port |
| `nodePort: 30007` | Required external node port |
| `selector: app: mysql` | Connects the service to MySQL Pods |

---

## Step 8: Verify PersistentVolume and PVC

After creating all the resources, I verified the storage configuration:

```bash
kubectl get pv
kubectl get pvc
```

Expected result:

- PersistentVolume: `mysql-pv`
- PersistentVolumeClaim: `mysql-pv-claim`
- Capacity: `250Mi`
- Status: `Bound`

---

## Step 9: Verify Kubernetes Secrets

I checked whether all three Secrets were created successfully:

```bash
kubectl get secrets
```

Expected Secret names:

```text
mysql-root-pass
mysql-user-pass
mysql-db-url
```

---

## Step 10: Verify MySQL Deployment and Pod

I checked the Deployment:

```bash
kubectl get deployments
```

Then checked the Pods:

```bash
kubectl get pods
```

The MySQL Pod should reach:

```text
STATUS: Running
READY: 1/1
```

If the Pod is not running, I checked its details:

```bash
kubectl describe pod <mysql-pod-name>
```

And inspected its logs:

```bash
kubectl logs <mysql-pod-name>
```

---

## Step 11: Verify MySQL Service

Finally, I checked the Service:

```bash
kubectl get services
```

The MySQL Service should show:

```text
NAME    TYPE       PORT(S)
mysql   NodePort   3306:30007/TCP
```

This confirms that the MySQL Service was created using the required NodePort.

---

# Kubernetes Architecture

The final architecture of this task:

```text
             Kubernetes Cluster
                     |
          PersistentVolume
             mysql-pv
              250Mi
                     |
                     |
          PersistentVolumeClaim
           mysql-pv-claim
              250Mi
                     |
                     |
              MySQL Pod
                     |
          mysql-deployment
                     |
              MySQL:5.7
                     |
             /var/lib/mysql
                     |
          Environment Variables
                     |
             Kubernetes Secrets
              /      |      \
             /       |       \
        Root Pass  User Pass  Database
                     |
                     |
               MySQL Service
                     |
                 NodePort
                   30007
```

---

# What I Learned

From this challenge, I learned and practiced:

- How to deploy a MySQL database on Kubernetes.
- How to create PersistentVolumes and PersistentVolumeClaims.
- How to connect persistent storage to a Pod.
- Why databases require persistent storage.
- How to create Kubernetes Secrets.
- How to store database credentials using Secrets.
- How to use `secretKeyRef` inside Deployment environment variables.
- How to create a Kubernetes Deployment.
- How to mount persistent storage at `/var/lib/mysql`.
- How to expose MySQL using a NodePort Service.
- How to use `kubectl apply` to deploy YAML configurations.
- How to verify PV, PVC, Secrets, Deployments, Pods, and Services.
- How different Kubernetes resources work together to deploy a stateful application.

# Challenge Status

Day 66 — Completed Successfully ✅

**MySQL deployed on Kubernetes with persistent storage, Kubernetes Secrets, and NodePort networking.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
