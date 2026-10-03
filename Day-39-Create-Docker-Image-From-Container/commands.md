# Day 39 - Commands

This file contains the commands used during **Day 39 of my 100 Days of DevOps Challenge**.

The objective was to create a Docker image from an existing Docker container.

---

# Step 1: List Docker Containers

List all Docker containers:

```bash
docker ps -a
```

This displays both running and stopped containers.

---

# Step 2: Inspect the Container

Inspect the required container:

```bash
docker inspect <container-name>
```

Example:

```bash
docker inspect my-container
```

---

# Step 3: Create an Image from the Container

Create a new Docker image using:

```bash
docker commit <container-name> <new-image-name>
```

Example:

```bash
docker commit my-container my-custom-image
```

---

# Step 4: Verify the New Image

List Docker images:

```bash
docker images
```

Or:

```bash
docker image ls
```

The newly created image should appear in the output.

---

# Step 5: Inspect the Created Image

Inspect the new image:

```bash
docker inspect <image-name>
```

Example:

```bash
docker inspect my-custom-image
```

---

# Step 6: Run a Container from the New Image

Create a new container from the image:

```bash
docker run -it <image-name>
```

Example:

```bash
docker run -it my-custom-image
```

---

# Quick Command Summary

```bash
# List all containers
docker ps -a

# Inspect the required container
docker inspect <container-name>

# Create image from container
docker commit <container-name> <new-image-name>

# Verify images
docker images

# Inspect the created image
docker inspect <image-name>

# Run a container from the new image
docker run -it <image-name>
```

---

# Main Command

The most important command for this challenge was:

```bash
docker commit <container-name> <new-image-name>
```

For example:

```bash
docker commit my-container my-custom-image
```

This creates a new Docker image from the current state of the existing container.

---

# Docker Workflow

Normal Docker workflow:

```text
Dockerfile
     ↓
Docker Image
     ↓
Docker Container
```

This challenge:

```text
Docker Container
     ↓
docker commit
     ↓
Docker Image
```

---

# Important Lesson

A Docker container is a running or stopped instance of an image, while a Docker image is a reusable template from which containers can be created.

The `docker commit` command allows the current state of a container to be captured as a new image:

```bash
docker commit <container-name> <image-name>
```

After creating the image, always verify it using:

```bash
docker images
```

**Day 39 completed successfully.** 
