# Day 36 - Commands

This file contains the commands used during **Day 36 of my 100 Days of DevOps Challenge**.

The objective was to create a Docker container named `nginx_3` using the `nginx:alpine` image.

---

# Step 1: Check Docker

Verify that Docker is installed:

```bash
docker --version
```

---

# Step 2: Create the Nginx Container

Create and start the container:

```bash
docker run -d --name nginx_3 nginx:alpine
```

---

# Step 3: Verify Running Containers

List running containers:

```bash
docker ps
```

---

# Step 4: Verify nginx_3

Check specifically for the required container:

```bash
docker ps --filter "name=nginx_3"
```

---

# Step 5: Verify the Docker Image

List Docker images:

```bash
docker images
```

The required image should be available:

```text
nginx    alpine
```

---

# Quick Command Summary

```bash
# Check Docker
docker --version

# Create and start nginx_3
docker run -d --name nginx_3 nginx:alpine

# List running containers
docker ps

# Verify nginx_3
docker ps --filter "name=nginx_3"

# List Docker images
docker images
```

---

# Understanding the Main Command

```bash
docker run -d --name nginx_3 nginx:alpine
```

The command can be broken down as:

```text
docker run
    ↓
Create and start a container

-d
    ↓
Run in detached/background mode

--name nginx_3
    ↓
Set the container name to nginx_3

nginx:alpine
    ↓
Use the nginx Alpine image
```

---

# Important Verification

The main verification command is:

```bash
docker ps
```

The output should show the container:

```text
nginx_3
```

The image should be:

```text
nginx:alpine
```

The container should have a running status.

---

# Important Lesson

For this challenge, no Dockerfile was required.

The container could be created directly from the existing Docker image using:

```bash
docker run -d --name nginx_3 nginx:alpine
```

This command creates and starts the required `nginx_3` container using the `nginx:alpine` image.

**Day 36 completed successfully.** 
