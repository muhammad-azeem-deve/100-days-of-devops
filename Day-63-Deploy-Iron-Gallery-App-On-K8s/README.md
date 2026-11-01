# Day 63 - Deploy Iron Gallery App on Kubernetes

## Challenge Overview

This is Day 63 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Kubernetes Application Deployment** and required deploying a customized Iron Gallery application stack on a Kubernetes cluster.

The application consisted of two components:

- **Frontend:** Iron Gallery application.
- **Database:** MariaDB database.

The objective was to create a dedicated Kubernetes namespace, deploy both components using Kubernetes Deployments, configure storage using `emptyDir` volumes, expose the frontend using a NodePort Service, and expose the database internally using a ClusterIP Service.

I used a **Declarative YAML Manifest Approach** by defining all the required Kubernetes resources in a single YAML file.

### Challenge Requirement

Deploy the Iron Gallery application on Kubernetes with the following specifications:

**1. Namespace**

Create a namespace named:

```text
iron-namespace-xfusion
```

**2. Frontend Deployment**

- Deployment name: `iron-gallery-deployment-xfusion`
- Container name: `iron-gallery-container-xfusion`
- Image: `kodekloud/irongallery:2.0`
- Replicas: `1`
- Label: `run: iron-gallery`
- CPU limit: `50m`
- Memory limit: `100Mi`
- Configure two `emptyDir` volumes:
  - `config` → `/usr/share/nginx/html/data`
  - `images` → `/usr/share/nginx/html/uploads`

**3. Database Deployment**

- Deployment name: `iron-db-deployment-xfusion`
- Container name: `iron-db-container-xfusion`
- Image: `kodekloud/irondb:2.0`
- Replicas: `1`
- Label: `db: mariadb`
- Configure the required MySQL environment variables.
- Configure an `emptyDir` volume mounted at `/var/lib/mysql`.

**4. Database Service**

- Service name: `iron-db-service-xfusion`
- Type: `ClusterIP`
- Port: `3306`
- Target Port: `3306`

**5. Frontend Service**

- Service name: `iron-gallery-service-xfusion`
- Type: `NodePort`
- Port: `80`
- Target Port: `80`
- NodePort: `32678`

---

## Objectives

The objectives of this challenge are:

- Understand Kubernetes namespaces.
- Create Deployments using YAML manifests.
- Deploy multiple application components.
- Configure container images and replicas.
- Apply CPU and memory limits.
- Configure environment variables for containers.
- Understand Kubernetes volumes and `emptyDir`.
- Configure volume mounts.
- Expose applications using Kubernetes Services.
- Understand the difference between ClusterIP and NodePort.
- Verify Pods, Deployments, Services, and Endpoints.
- Access the frontend application through NodePort.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 63 |
| Technology | Kubernetes |
| Namespace | `iron-namespace-xfusion` |
| Frontend Image | `kodekloud/irongallery:2.0` |
| Database Image | `kodekloud/irondb:2.0` |
| Frontend Replicas | 1 |
| Database Replicas | 1 |
| Frontend Service | NodePort |
| Database Service | ClusterIP |
| NodePort | `32678` |
| Status | Completed |

---

# Solution

## Step 1: Access the Kubernetes Cluster

First, I accessed the Kubernetes environment through the KodeKloud jump host.

The `kubectl` command was already configured to communicate with the Kubernetes cluster.

I verified the available nodes:

```bash
kubectl get nodes
```

I also checked the existing namespaces:

```bash
kubectl get namespaces
```

---

## Step 2: Create the YAML Manifest File

Instead of creating each resource separately, I decided to use a single YAML manifest.

I created the file:

```bash
nano iron-gallery.yaml
```

This file contained five Kubernetes resources:

1. Namespace
2. Frontend Deployment
3. Database Deployment
4. Database Service
5. Frontend Service

I separated each resource using three hyphens:

```yaml
---
```

This allowed Kubernetes to process multiple resources from one file.

---

## Step 3: Create the Namespace

The first resource was the Namespace:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: iron-namespace-xfusion
```

A namespace provides logical isolation for Kubernetes resources.

Both Deployments and Services were created inside this namespace.

---

## Step 4: Create the Frontend Deployment

I configured the frontend Deployment with:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-gallery-deployment-xfusion
  namespace: iron-namespace-xfusion
  labels:
    run: iron-gallery
spec:
  replicas: 1
  selector:
    matchLabels:
      run: iron-gallery
  template:
    metadata:
      labels:
        run: iron-gallery
    spec:
      containers:
        - name: iron-gallery-container-xfusion
          image: kodekloud/irongallery:2.0
          resources:
            limits:
              memory: "100Mi"
              cpu: "50m"
          volumeMounts:
            - name: config
              mountPath: /usr/share/nginx/html/data
            - name: images
              mountPath: /usr/share/nginx/html/uploads
      volumes:
        - name: config
          emptyDir: {}
        - name: images
          emptyDir: {}
```

Important configurations:

- One frontend replica.
- Correct Docker image and container name.
- CPU limit of `50m`.
- Memory limit of `100Mi`.
- Two `emptyDir` volumes.
- Two corresponding volume mounts.

---

## Step 5: Create the Database Deployment

Next, I configured the MariaDB Deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-db-deployment-xfusion
  namespace: iron-namespace-xfusion
  labels:
    db: mariadb
spec:
  replicas: 1
  selector:
    matchLabels:
      db: mariadb
  template:
    metadata:
      labels:
        db: mariadb
    spec:
      containers:
        - name: iron-db-container-xfusion
          image: kodekloud/irondb:2.0
          env:
            - name: MYSQL_DATABASE
              value: "database_host"
            - name: MYSQL_ROOT_PASSWORD
              value: "Root@2026#Strong"
            - name: MYSQL_PASSWORD
              value: "Admin@2026#Strong"
            - name: MYSQL_USER
              value: "admin"
          volumeMounts:
            - name: db
              mountPath: /var/lib/mysql
      volumes:
        - name: db
          emptyDir: {}
```

Important configurations:

- One database replica.
- Correct MariaDB image.
- Required MySQL environment variables.
- Custom database user.
- Database storage using `emptyDir`.
- Volume mounted at `/var/lib/mysql`.

The example passwords are for demonstration. They should not be reused as production credentials or exposed in public repositories.

---

## Step 6: Create the Database Service

I created a ClusterIP Service to expose MariaDB internally:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: iron-db-service-xfusion
  namespace: iron-namespace-xfusion
spec:
  type: ClusterIP
  selector:
    db: mariadb
  ports:
    - protocol: TCP
      port: 3306
      targetPort: 3306
```

ClusterIP allows other workloads inside the Kubernetes cluster to access the database through its Service.

---

## Step 7: Create the Frontend Service

Next, I created a NodePort Service to expose the frontend:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: iron-gallery-service-xfusion
  namespace: iron-namespace-xfusion
spec:
  type: NodePort
  selector:
    run: iron-gallery
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 32678
```

The NodePort configuration allows access to the frontend through port `32678` on a Kubernetes node.

---

## Step 8: Apply the YAML Manifest

After completing the YAML file, I deployed all resources using:

```bash
kubectl apply -f iron-gallery.yaml
```

This command created the Namespace, Deployments, and Services defined in the manifest.

---

## Step 9: Verify the Namespace

I verified that the namespace was created successfully:

```bash
kubectl get namespaces
```

Expected namespace:

```text
iron-namespace-xfusion
```

---

## Step 10: Verify the Deployments

I checked both Deployments:

```bash
kubectl get deployments -n iron-namespace-xfusion
```

Expected result:

```text
NAME                              READY   UP-TO-DATE   AVAILABLE
iron-gallery-deployment-xfusion   1/1     1            1
iron-db-deployment-xfusion        1/1     1            1
```

Both Deployments should have one available replica.

---

## Step 11: Verify the Pods

I checked the Pods using:

```bash
kubectl get pods -n iron-namespace-xfusion
```

Both Pods should eventually reach:

```text
READY   STATUS
1/1     Running
```

This confirms that the frontend and database containers have started successfully.

---

## Step 12: Verify Kubernetes Services

I checked the Services:

```bash
kubectl get services -n iron-namespace-xfusion
```

Expected configuration:

| Service | Type | Port |
|---------|------|------|
| iron-db-service-xfusion | ClusterIP | 3306/TCP |
| iron-gallery-service-xfusion | NodePort | 80:32678/TCP |

---

## Step 13: Verify Service Endpoints

I checked whether the Services were connected to their respective Pods:

```bash
kubectl get endpoints -n iron-namespace-xfusion
```

The Services should have endpoints when their matching Pods are ready.

The frontend Service uses:

```yaml
selector:
  run: iron-gallery
```

The database Service uses:

```yaml
selector:
  db: mariadb
```

These selectors must match the labels defined in their respective Pod templates.

---

## Step 14: Access the Iron Gallery Application

Finally, I accessed the frontend through the NodePort Service.

The URL format was:

```text
http://<KUBERNETES-NODE-IP>:32678
```

The Iron Gallery installation page loaded successfully.

**Note:** The challenge did not require establishing a connection between the frontend and database. Loading the installation page was sufficient to satisfy the success criteria.

---

# Important Kubernetes Concepts

## 1. Namespace

A namespace logically separates Kubernetes resources.

```bash
kubectl get namespaces
```

## 2. Deployment

A Deployment manages application Pods and maintains the required replica count.

```bash
kubectl get deployments -n iron-namespace-xfusion
```

## 3. emptyDir Volume

An `emptyDir` volume provides temporary storage shared through the Pod's containers.

Its data is removed when the Pod is deleted.

## 4. ClusterIP

ClusterIP exposes a Service internally within the Kubernetes cluster.

In this challenge, it was used for MariaDB on port `3306`.

## 5. NodePort

NodePort exposes a Service through a port on the Kubernetes nodes.

In this challenge, the frontend was exposed through port `32678`.

---

# What I Learned

From this challenge, I learned and practiced:

- How to deploy a multi-component application on Kubernetes.
- How to create and use namespaces.
- How to configure Kubernetes Deployments.
- How to deploy frontend and database containers.
- How to configure CPU and memory limits.
- How to pass environment variables to containers.
- How to configure `emptyDir` volumes and volume mounts.
- How to expose applications through Kubernetes Services.
- The difference between NodePort and ClusterIP.
- How Kubernetes labels and selectors connect Services to Pods.
- How to deploy multiple resources using one YAML manifest.
- How to use `kubectl apply` for declarative deployments.
- How to verify Pods, Deployments, Services, and Endpoints.
- How to access a Kubernetes application through NodePort.

# Challenge Status

Day 63 — Completed Successfully ✅

**Deployed the Iron Gallery frontend and MariaDB database on Kubernetes using a single declarative YAML manifest.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
