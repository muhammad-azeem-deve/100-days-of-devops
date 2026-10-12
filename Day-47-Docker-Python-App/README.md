# Day 47 - Deploy a Python App Using Dockerfile

## Challenge Overview

This is Day 47 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Docker and Python Application Deployment**. The objective was to deploy a Python application using a Dockerfile, build a Docker image, run the container, and verify that the application was working correctly.

### Challenge Requirement

Deploy a Python application inside a Docker container using a Dockerfile.

The task involved creating or using the Python application, writing a Dockerfile, building the Docker image, running the container, and verifying the application output and container status.

---

## Objectives

The objectives of this challenge are:

- Understand how Docker runs Python applications.
- Create or use a Python application.
- Write a Dockerfile.
- Build a Docker image.
- Run a Docker container.
- Verify the container and application output.
- Understand the difference between a Docker image and a container.
- Learn basic Docker troubleshooting commands.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 47 |
| Application | Python |
| Containerization Tool | Docker |
| Build File | `Dockerfile` |
| Image Name | `python-app` (example) |
| Container Name | `python-container` (example) |
| Status | Documented |

---

# Solution

## Step 1: Verify Docker Installation

First, I checked whether Docker was available on the server.

```bash
docker --version
```

This command displays the installed Docker version.

## Step 2: Create the Project Directory

I created a directory for the Python application.

```bash
mkdir python-app
cd python-app
```

## Step 3: Create the Python Application

I created a Python file named `app.py`.

```bash
vi app.py
```

For the simple example, I used the following code:

```python
print("Hello from Python running inside Docker!")
```

This application prints a message when executed.

## Step 4: Create the Dockerfile

I created a file named `Dockerfile`.

```bash
vi Dockerfile
```

I added the following instructions:

```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY app.py .
CMD ["python", "app.py"]
```

### Explanation

- `FROM` selects the Python base image.
- `WORKDIR` sets the working directory inside the container.
- `COPY` copies the application file into the image.
- `CMD` specifies the command to execute when the container starts.

## Step 5: Build the Docker Image

I built the Docker image using:

```bash
docker build -t python-app .
```

The `-t` option assigns a name to the image, and the `.` specifies the current directory as the build context.

I verified the image using:

```bash
docker images
```

## Step 6: Run the Docker Container

After building the image, I created and ran a container:

```bash
docker run --name python-container python-app
```

The expected output for the example application was:

```text
Hello from Python running inside Docker!
```

Because this example prints a message and exits, its container stops after the Python process finishes.

## Step 7: Verify the Container

I checked the container status using:

```bash
docker ps -a
```

This command lists running and stopped containers.

I checked the application output using:

```bash
docker logs python-container
```

I also checked the container exit code:

```bash
docker inspect python-container --format='{{.State.ExitCode}}'
```

An exit code of `0` indicates that the process completed successfully.

---

# Dockerfile Workflow

The deployment process follows this sequence:

1. Create or obtain the Python application.
2. Write the Dockerfile.
3. Build the Docker image.
4. Run the container.
5. Check the container status.
6. Verify the application output.

---

# What I Learned

From this challenge, I learned and practiced:

- How Docker can package a Python application.
- How to write a basic Dockerfile.
- How to select a Python base image.
- How to copy application files into a container image.
- How to build an image using `docker build`.
- How to run a container using `docker run`.
- How to list images and containers.
- How to verify application output using `docker logs`.
- How to inspect a container's exit code.
- The difference between an image and a container.
- Why a container may stop after a short-running application finishes.

# Challenge Status

Day 47 — Documentation Prepared

**Python + Dockerfile + Docker Image + Container Verification**

100 Days. 100 Challenges. One DevOps Journey.

I will continue documenting each challenge.
