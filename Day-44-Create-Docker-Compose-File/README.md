# Day 44 - Run Apache Container Using Docker Compose

## Challenge Overview

This is Day 44 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Docker Compose** and required creating a Docker Compose file to run an Apache HTTP server container.

The task required using the `httpd:latest` Docker image and mapping **host port 8087** to **container port 80**.

### Challenge Requirement

Create a Docker Compose file that:

- Runs an Apache HTTP server container.
- Uses the `httpd:latest` image.
- Maps host port `8087` to container port `80`.
- Starts the container using Docker Compose.

---

## Objectives

The objectives of this challenge are:

- Understand the purpose of Docker Compose.
- Create a `docker-compose.yml` file.
- Define a Docker service.
- Use the `httpd:latest` image.
- Map a host port to a container port.
- Start the container using Docker Compose.
- Verify that the container is running.
- Verify that Apache is accessible through port `8087`.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 44 |
| Technology | Docker Compose |
| Image | `httpd:latest` |
| Host Port | `8087` |
| Container Port | `80` |
| Container | Apache HTTP Server |
| Status | Completed |

---

# Solution

## Step 1: Check Docker

First, I verified that Docker was installed and available on the server.

```bash
docker --version
```

---

## Step 2: Check Docker Compose

I also verified Docker Compose:

```bash
docker compose version
```

If the environment uses the older Docker Compose command, it can be checked using:

```bash
docker-compose --version
```

---

## Step 3: Create the Docker Compose File

I created a file named:

```text
docker-compose.yml
```

The file contains:

```yaml
services:
  web:
    image: httpd:latest
    ports:
      - "8087:80"
```

### Explanation

The `services` section defines the containers that Docker Compose will run.

```yaml
services:
```

I created one service called:

```yaml
web:
```

The Apache image is specified using:

```yaml
image: httpd:latest
```

The port mapping is:

```yaml
ports:
  - "8087:80"
```

This means:

```text
Host Port      Container Port
   8087   --->      80
```

---

## Step 4: Start the Container

After creating the Compose file, I started the container in detached mode:

```bash
docker compose up -d
```

The `-d` option runs the container in the background.

---

## Step 5: Verify the Running Container

I checked the running containers using:

```bash
docker ps
```

The port mapping should show something similar to:

```text
0.0.0.0:8087->80/tcp
```

This confirms that host port `8087` is mapped to container port `80`.

---

## Step 6: Test Apache

I tested the Apache web server using:

```bash
curl http://localhost:8087
```

If Apache is running correctly, it should return the Apache HTTP server HTML response.

The application can also be accessed through:

```text
http://<server-ip>:8087
```

---

## Step 7: Check Docker Compose Services

I verified the Compose service using:

```bash
docker compose ps
```

This displays the service, container status, and port mapping.

---

# Understanding Port Mapping

The most important part of this challenge is:

```yaml
ports:
  - "8087:80"
```

Docker uses the format:

```text
HOST_PORT:CONTAINER_PORT
```

Therefore:

```text
8087:80
```

means:

```text
Linux Server                    Docker Container
     |                                |
     | Port 8087                      | Port 80
     |                                |
     └──────────────>─────────────────┘
```

When a user accesses:

```text
http://<server-ip>:8087
```

Docker forwards the request to port `80` inside the Apache container.

---

# Docker Compose File

The final `docker-compose.yml` file was:

```yaml
services:
  web:
    image: httpd:latest
    ports:
      - "8087:80"
```

---

# What I Learned

From this challenge, I learned and practiced:

- What Docker Compose is used for.
- How to create a Docker Compose file.
- How to define a service in Compose.
- How to use the `httpd:latest` Docker image.
- How Docker port mapping works.
- The difference between a host port and a container port.
- How to start containers using `docker compose up -d`.
- How to verify containers using `docker ps`.
- How to verify Compose services using `docker compose ps`.
- How to test an Apache container using `curl`.

# Challenge Status

Day 44 — Completed Successfully 

**Docker Compose + Apache + Port Mapping — another step forward in my DevOps journey.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
