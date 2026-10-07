# Day 43 - Docker Port Binding Commands

This file contains the commands used during **Day 43 of my 100 Days of DevOps Challenge**.

The objective was to pull the `nginx:stable` image and run a container with host port `8085` mapped to container port `80`.

---

# Step 1: Pull Nginx Stable Image

```bash
docker pull nginx:stable
```

---

# Step 2: Verify Docker Image

```bash
docker images
```

Verify that the following image is available:

```text
nginx
```

with the tag:

```text
stable
```

---

# Step 3: Run Nginx Container with Port Binding

```bash
docker run -d --name nginx-container -p 8085:80 nginx:stable
```

Port mapping:

```text
8085:80
```

Meaning:

```text
Host Port 8085 → Container Port 80
```

---

# Step 4: Verify Running Container

```bash
docker ps
```

Look for:

```text
0.0.0.0:8085->80/tcp
```

---

# Step 5: Check Container Port Mapping

```bash
docker port nginx-container
```

Expected output:

```text
80/tcp -> 0.0.0.0:8085
```

---

# Step 6: Test Nginx

Test the Nginx server from the Docker host:

```bash
curl http://localhost:8085
```

The command should return the Nginx welcome page HTML.

---

# Step 7: Check Container Logs

If there is a problem with the Nginx container, check its logs:

```bash
docker logs nginx-container
```

---

# Step 8: Check Container Details

To inspect the container configuration:

```bash
docker inspect nginx-container
```

---

# Quick Command Summary

```bash
# Pull Nginx stable image
docker pull nginx:stable

# Verify image
docker images

# Run Nginx container and bind port 8085 to container port 80
docker run -d --name nginx-container -p 8085:80 nginx:stable

# Verify running container
docker ps

# Verify port mapping
docker port nginx-container

# Test Nginx
curl http://localhost:8085

# Check container logs if required
docker logs nginx-container

# Inspect container
docker inspect nginx-container
```

---

# Understanding the Port Binding

The important command in this challenge was:

```bash
docker run -d --name nginx-container -p 8085:80 nginx:stable
```

The port option:

```text
-p 8085:80
```

means:

```text
8085 = Host Port
80   = Container Port
```

Therefore:

```text
Host:8085
     │
     ▼
Container:80
     │
     ▼
   Nginx
```

To access the application locally:

```bash
curl http://localhost:8085
```

Or from another machine:

```text
http://<server-ip>:8085
```

---

# Important Lesson

Docker port binding follows this format:

```text
-p HOST_PORT:CONTAINER_PORT
```

For this challenge:

```text
-p 8085:80
```

means that traffic arriving at **host port 8085** is forwarded to **port 80 inside the Nginx container**.

**Day 43 completed successfully.** 
