# Day 36 - Create Nginx Docker Container

## Challenge Overview

This is Day 36 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Docker Containers** and required creating a Docker container with a specific name using the `nginx:alpine` image.

### Challenge Requirement

Create a Docker container with the following requirements:

* Container name: `nginx_3`
* Docker image: `nginx:alpine`

The objective was to create and start the container and then verify that it was running with the correct name and image.

---

## Objectives

The objectives of this challenge are:

* Understand the difference between a Docker image and container.
* Create a Docker container using an existing image.
* Give a custom name to a Docker container.
* Run the container in detached mode.
* Verify the running container.
* Verify that the correct Docker image is being used.

---

## Environment

| Item             | Details            |
| ---------------- | ------------------ |
| Challenge        | 100 Days of DevOps |
| Platform         | KodeKloud          |
| Day              | 36                 |
| Container Name   | `nginx_3`          |
| Image            | `nginx:alpine`     |
| Container Status | Running            |
| Status           | Completed          |

---

# Solution

## Step 1: Check Docker Installation

First, I verified that Docker was installed on the server:

```bash
docker --version
```

---

## Step 2: Create the Docker Container

I created the required container using the `nginx:alpine` image:

```bash
docker run -d --name nginx_3 nginx:alpine
```

### Understanding the Command

```text
docker run
```

Creates and starts a new Docker container.

```text
-d
```

Runs the container in detached/background mode.

```text
--name nginx_3
```

Assigns the name `nginx_3` to the container.

```text
nginx:alpine
```

Specifies the Docker image to use.

---

## Step 3: Verify the Container

After creating the container, I verified that it was running:

```bash
docker ps
```

The container should appear with:

```text
NAME
nginx_3
```

---

## Step 4: Verify the Specific Container

I also checked specifically for the `nginx_3` container:

```bash
docker ps --filter "name=nginx_3"
```

This confirmed that the required container was running.

---

## Step 5: Verify the Docker Image

I checked the locally available Docker images:

```bash
docker images
```

The output should contain:

```text
nginx    alpine
```

This confirmed that the container was created using the required `nginx:alpine` image.

---

# What I Learned

From this challenge, I learned and practiced:

* How Docker containers are created from images.
* How to use the `docker run` command.
* How to assign a custom name to a container.
* How to run a container in detached mode using `-d`.
* How Docker automatically pulls an image if it is not available locally.
* How to verify running containers using `docker ps`.
* How to verify a specific container using Docker filters.
* The difference between a Docker image and a Docker container.

# Challenge Status

Day 36 — Completed Successfully 

**Created the `nginx_3` container successfully using the `nginx:alpine` image.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
