# Day 43 - Docker Port Binding

## Challenge Overview

This is Day 43 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Docker Port Binding** and required pulling the `nginx:stable` Docker image and running a container using that image.

The container's Nginx service listens on port `80`, and the task required binding the **host port `8085`** to the **container port `80`**.

### Challenge Requirement

Pull the `nginx:stable` image and run a container using that image with the following port mapping:

```text
Host Port: 8085
Container Port: 80
```

The required port binding was:

```text
8085:80
```

---

## Objectives

The objectives of this challenge are:

- Pull the `nginx:stable` Docker image.
- Create and run an Nginx container.
- Bind host port `8085` to container port `80`.
- Verify that the container is running.
- Verify the Docker port mapping.
- Test the Nginx web server using the mapped host port.
- Understand Docker port binding.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 43 |
| Technology | Docker |
| Image | `nginx:stable` |
| Container | Nginx |
| Host Port | `8085` |
| Container Port | `80` |
| Port Binding | `8085:80` |
| Status | Completed |

---

# Solution

## Step 1: Pull the Nginx Stable Image

First, I pulled the required `nginx:stable` image from Docker Hub:

```bash
docker pull nginx:stable
```

This downloads the stable Nginx image to the local Docker host.

---

## Step 2: Verify the Docker Image

After pulling the image, I verified that it was available locally:

```bash
docker images
```

The output should contain:

```text
nginx
```

with the tag:

```text
stable
```

---

## Step 3: Run the Nginx Container with Port Binding

Next, I created and started the Nginx container.

The required port mapping was:

```text
8085:80
```

I used:

```bash
docker run -d --name nginx-container -p 8085:80 nginx:stable
```

### Understanding the Command

```text
docker run
```

Creates and starts a new container.

```text
-d
```

Runs the container in detached mode.

```text
--name nginx-container
```

Assigns the name `nginx-container` to the container.

```text
-p 8085:80
```

Maps:

```text
Host Port 8085 → Container Port 80
```

```text
nginx:stable
```

Specifies the image and tag used to create the container.

---

## Step 4: Verify the Running Container

I checked the running containers using:

```bash
docker ps
```

The port mapping should show something similar to:

```text
0.0.0.0:8085->80/tcp
```

This confirms that host port `8085` is mapped to container port `80`.

---

## Step 5: Test the Nginx Server

I tested the Nginx server using:

```bash
curl http://localhost:8085
```

If the container is working correctly, the command returns the Nginx welcome page HTML.

---

## Step 6: Verify the Port Mapping

The port mapping can also be checked using:

```bash
docker port nginx-container
```

Expected output:

```text
80/tcp -> 0.0.0.0:8085
```

This confirms that Nginx's container port `80` is accessible through host port `8085`.

---

# Understanding Docker Port Binding

Docker uses the following syntax for port binding:

```text
-p HOST_PORT:CONTAINER_PORT
```

For this challenge:

```text
-p 8085:80
```

means:

```text
Host
Port 8085
   │
   ▼
Docker Container
Port 80
   │
   ▼
Nginx
```

Therefore, a request to:

```text
http://<server-ip>:8085
```

is forwarded to:

```text
Container Port 80
```

where Nginx is listening.

---

# What I Learned

From this challenge, I learned and practiced:

- How to pull a Docker image using `docker pull`.
- How Docker image tags work.
- How to run a container using `docker run`.
- How to run containers in detached mode.
- How to assign a custom container name.
- How Docker port binding works.
- How to map a host port to a container port.
- How to verify port mappings using `docker ps`.
- How to use `docker port` to inspect container port mappings.
- How to test a containerized web server using `curl`.

# Challenge Status

Day 43 — Completed Successfully 

**Nginx container running successfully with host port `8085` mapped to container port `80`.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
