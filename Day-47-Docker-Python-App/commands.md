# Day 47 - Commands

This file contains the commands for **Day 47 of my 100 Days of DevOps Challenge by KodeKloud**.

The objective was to deploy a Python application using a Dockerfile, run the container, and verify its output.

---

## Step 1: Verify Docker

```bash
docker --version
```

---

## Step 2: Create the Project Directory

```bash
mkdir python-app
cd python-app
```

---

## Step 3: Create the Python Application

```bash
vi app.py
```

Example application:

```python
print("Hello from Python running inside Docker!")
```

---

## Step 4: Create the Dockerfile

```bash
vi Dockerfile
```

Example Dockerfile:

```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY app.py .
CMD ["python", "app.py"]
```

---

## Step 5: Build the Docker Image

```bash
docker build -t python-app .
```

Verify the image:

```bash
docker images
```

---

## Step 6: Run the Container

```bash
docker run --name python-container python-app
```

---

## Step 7: Verify the Container

List all containers:

```bash
docker ps -a
```

Check the application output:

```bash
docker logs python-container
```

Check the exit code:

```bash
docker inspect python-container --format='{{.State.ExitCode}}'
```

---

## Step 8: Troubleshooting

Check Docker service status on a systemd-based Linux server:

```bash
sudo systemctl status docker
```

If Docker is installed but not running, start it:

```bash
sudo systemctl start docker
```

View the container's output and errors:

```bash
docker logs python-container
```

View detailed container information:

```bash
docker inspect python-container
```

If you need to rerun the example container, remove the old container first:

```bash
docker rm python-container
```

Then run it again:

```bash
docker run --name python-container python-app
```

**Note:** Removing a container does not remove its image.

---

## Quick Command Summary

```bash
# Check Docker
docker --version

# Create project
mkdir python-app
cd python-app

# Create application
vi app.py

# Create Dockerfile
vi Dockerfile

# Build image
docker build -t python-app .

# Verify image
docker images

# Run container
docker run --name python-container python-app

# Verify containers
docker ps -a

# Check application output
docker logs python-container

# Check exit code
docker inspect python-container --format='{{.State.ExitCode}}'
```

---

## Important Lesson

The key commands in this challenge are:

```bash
docker build -t python-app .
docker run --name python-container python-app
docker ps -a
docker logs python-container
```

The Dockerfile defines how the application is packaged, `docker build` creates the image, and `docker run` creates and starts a container.

Always verify the actual application output and container status before considering a deployment successful.
