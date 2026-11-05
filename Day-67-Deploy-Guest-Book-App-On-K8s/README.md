# Day 67 - Deploy Guestbook Application on Kubernetes

## Challenge Overview

This is Day 67 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Kubernetes Application Deployment** and required deploying a complete Guestbook application using a PHP frontend and Redis backend.

The application architecture consisted of two infrastructure layers:

- **Frontend Tier:** PHP application responsible for displaying the Guestbook interface and accepting visitor messages.
- **Backend Tier:** Redis Master and Redis Slave deployments responsible for storing and retrieving guestbook entries.

The objective was to create Kubernetes Deployments and Services using YAML manifests, configure the required replicas and resource requests, establish communication through Kubernetes DNS, and expose the frontend using a NodePort Service.

### Challenge Requirement

Deploy the Guestbook application on the Kubernetes cluster with the following components:

**Backend Tier**

1. Create a Redis Master Deployment named `redis-master` with 1 replica.
2. Use the `redis` image and container name `master-redis-devops`.
3. Configure CPU request to `100m` and memory request to `100Mi`.
4. Expose Redis on port `6379`.
5. Create a ClusterIP Service named `redis-master`.
6. Create a Redis Slave Deployment named `redis-slave` with 2 replicas.
7. Use the `gcr.io/google_samples/gb-redisslave:v3` image and container name `slave-redis-devops`.
8. Configure CPU and memory requests to `100m` and `100Mi`.
9. Set `GET_HOSTS_FROM=dns`.
10. Create a ClusterIP Service named `redis-slave` on port `6379`.

**Frontend Tier**

1. Create a Deployment named `frontend` with 3 replicas.
2. Use the PHP frontend image:

   `gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff`

3. Use container name `php-redis-devops`.
4. Configure CPU request to `100m` and memory request to `100Mi`.
5. Set `GET_HOSTS_FROM=dns`.
6. Expose container port `80`.
7. Create a NodePort Service named `frontend`.
8. Configure Service port `80` and NodePort `30009`.

---

## Objectives

The objectives of this challenge are:

- Understand Kubernetes multi-tier application architecture.
- Create Deployments using YAML manifests.
- Deploy Redis Master and Redis Slave components.
- Deploy a PHP-based Guestbook frontend.
- Configure the required number of replicas.
- Create Kubernetes Services for internal and external communication.
- Understand labels and selectors.
- Configure environment variables for Kubernetes DNS service discovery.
- Configure CPU and memory resource requests.
- Expose the frontend using NodePort.
- Verify Pods, Deployments, and Services using `kubectl`.
- Test the Guestbook application through the KodeKloud App interface.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 67 |
| Technology | Kubernetes |
| Application | Guestbook |
| Frontend | PHP |
| Backend | Redis |
| Redis Master Replicas | 1 |
| Redis Slave Replicas | 2 |
| Frontend Replicas | 3 |
| Total Pods | 6 |
| Redis Port | 6379 |
| Frontend Port | 80 |
| NodePort | 30009 |
| Status | Completed |

---

# Solution

## Step 1: Verify Kubernetes Cluster

First, I accessed the KodeKloud jump host, where `kubectl` was already configured to communicate with the Kubernetes cluster.

I verified the available nodes:

```bash
kubectl get nodes
```

This confirmed that the Kubernetes cluster was accessible.

## Step 2: Create the Project Directory

I created a directory to organize the Guestbook deployment files.

```bash
mkdir guestbook
cd guestbook
```

Then I created the Kubernetes YAML manifest:

```bash
nano guestbook.yaml
```

## Step 3: Create Redis Master Deployment and Service

First, I configured the Redis Master Deployment.

The Deployment was named `redis-master` and configured with one replica.

Important configurations:

- Image: `redis`
- Container name: `master-redis-devops`
- Replicas: `1`
- CPU request: `100m`
- Memory request: `100Mi`
- Container port: `6379`

I also created a ClusterIP Service named `redis-master` to allow internal communication with the Redis Master.

## Step 4: Create Redis Slave Deployment and Service

Next, I created the Redis Slave Deployment.

Important configurations:

- Deployment name: `redis-slave`
- Container name: `slave-redis-devops`
- Image: `gcr.io/google_samples/gb-redisslave:v3`
- Replicas: `2`
- CPU request: `100m`
- Memory request: `100Mi`
- Container port: `6379`
- Environment variable: `GET_HOSTS_FROM=dns`

I created a ClusterIP Service named `redis-slave` to provide internal access to the Redis Slave Pods.

The Redis Slaves replicate data from the master and support read operations.

## Step 5: Create Frontend Deployment

After configuring the backend, I created the PHP frontend Deployment.

Important configurations:

- Deployment name: `frontend`
- Container name: `php-redis-devops`
- Replicas: `3`
- PHP frontend image: `gcr.io/google-samples/gb-frontend`
- CPU request: `100m`
- Memory request: `100Mi`
- Container port: `80`
- Environment variable: `GET_HOSTS_FROM=dns`

The frontend communicates with Redis through Kubernetes Services.

## Step 6: Create Frontend NodePort Service

Next, I created a Service named `frontend`.

The Service was configured with:

```yaml
type: NodePort
ports:
  - port: 80
    targetPort: 80
    nodePort: 30009
```

This exposed the Guestbook frontend outside the cluster through NodePort `30009`.

## Step 7: Apply Kubernetes YAML Manifest

After configuring all three Deployments and their corresponding Services, I applied the manifest:

```bash
kubectl apply -f guestbook.yaml
```

This created the required Kubernetes resources.

## Step 8: Verify Deployments

I checked whether all Deployments were created successfully:

```bash
kubectl get deployments
```

Expected replica configuration:

| Deployment | Desired Replicas |
|------------|-----------------:|
| redis-master | 1 |
| redis-slave | 2 |
| frontend | 3 |

I also checked rollout status:

```bash
kubectl rollout status deployment/redis-master
kubectl rollout status deployment/redis-slave
kubectl rollout status deployment/frontend
```

## Step 9: Verify Pods

I checked the status of all application Pods:

```bash
kubectl get pods -o wide
```

The expected result was six Pods:

- 1 Redis Master Pod
- 2 Redis Slave Pods
- 3 Frontend Pods

All Pods needed to reach the `Running` and `Ready` state.

## Step 10: Verify Kubernetes Services

I checked all Services:

```bash
kubectl get services
```

Expected Service configuration:

| Service | Type | Port |
|---------|------|------|
| redis-master | ClusterIP | 6379 |
| redis-slave | ClusterIP | 6379 |
| frontend | NodePort | 80:30009 |

## Step 11: Verify Labels and Selectors

I checked the labels assigned to the Pods:

```bash
kubectl get pods --show-labels
```

I also inspected the Services:

```bash
kubectl describe service redis-master
kubectl describe service redis-slave
kubectl describe service frontend
```

This helped verify that the Services were selecting the correct Pods through their labels and selectors.

## Step 12: Access and Test the Guestbook Application

Finally, I opened the application using the **App** button provided by KodeKloud.

I verified that the Guestbook interface was accessible.

I tested the application by:

1. Opening the Guestbook interface.
2. Entering a visitor name.
3. Writing a guestbook message.
4. Submitting the message.
5. Verifying that the submitted message appeared in the application.

This confirmed that the frontend and Redis backend components were communicating correctly.

---

# Application Architecture

The application consisted of the following components:

| Component | Replicas | Responsibility |
|-----------|----------|----------------|
| PHP Frontend | 3 | User interface and visitor messages |
| Redis Master | 1 | Handles write operations |
| Redis Slaves | 2 | Handle read operations |

### Application Communication Flow

```text
              USER
                |
                v
       FRONTEND SERVICE
         NodePort 30009
                |
                v
        FRONTEND DEPLOYMENT
           3 PHP PODS
                |
                v
       KUBERNETES DNS
                |
          +-----+-----+
          |           |
          v           v
    REDIS MASTER   REDIS SLAVE
      SERVICE       SERVICE
          |           |
          v           v
      1 MASTER      2 SLAVES
        PODS          PODS
```

---

# Important Kubernetes Concepts Learned

### 1. Deployments

A Deployment manages the desired number of Pods and helps maintain application availability.

### 2. Services

Services provide stable network access to Pods.

- ClusterIP is used for internal communication.
- NodePort exposes an application through a port on Kubernetes nodes.

### 3. Labels and Selectors

Labels identify Kubernetes resources, while selectors help Services and Deployments identify the Pods they manage.

### 4. Environment Variables

The following environment variable enables DNS-based service discovery:

```yaml
env:
  - name: GET_HOSTS_FROM
    value: dns
```

### 5. Resource Requests

The application containers were configured with:

```yaml
resources:
  requests:
    cpu: 100m
    memory: 100Mi
```

This defines the minimum compute resources requested by each container.

---

# What I Learned

From this challenge, I learned and practiced:

- How to deploy a complete multi-tier application on Kubernetes.
- How to create Kubernetes Deployments using YAML.
- How to deploy Redis Master and Redis Slave components.
- How to deploy a PHP frontend application.
- How to configure multiple replicas.
- How to create ClusterIP and NodePort Services.
- How Kubernetes labels and selectors connect resources.
- How Kubernetes DNS provides service discovery.
- How to configure environment variables inside containers.
- How to configure CPU and memory requests.
- How to expose applications outside a Kubernetes cluster.
- How to verify Deployments, Pods, and Services using `kubectl`.
- How to test communication between frontend and backend components.

# Challenge Status

Day 67 — Completed Successfully ✅

**Successfully deployed a multi-tier Guestbook application on Kubernetes using PHP, Redis Master, and Redis Slave components.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
