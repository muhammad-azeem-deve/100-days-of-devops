# Day 41 - Create a Dockerfile

## Challenge Overview

This is Day 41 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Docker and Dockerfile creation**. The objective was to create a Dockerfile according to the requirements provided in the KodeKloud lab.

A Dockerfile contains a set of instructions that Docker uses to build a Docker image.

### Challenge Requirement

Create a **Dockerfile** according to the requirements provided by the KodeKloud challenge.

The Dockerfile acts as a blueprint for building a Docker image.

---

## Objectives

The objectives of this challenge are:

- Understand what a Dockerfile is.
- Create a Dockerfile.
- Understand Dockerfile instructions.
- Use a base image with the `FROM` instruction.
- Understand how Docker uses a Dockerfile to build an image.
- Verify that the Dockerfile exists in the required location.
- Understand the basic workflow of Docker image creation.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 41 |
| Topic | Dockerfile |
| File | `Dockerfile` |
| Status | Completed |

---

# Solution

## Step 1: Access the Server

First, I accessed the required server using SSH.

The SSH command format is:

```bash
ssh <username>@<server-hostname>
```

After connecting, I verified the server:

```bash
hostname
```

---

## Step 2: Navigate to the Required Directory

I moved to the directory where the Dockerfile needed to be created:

```bash
cd <required-directory>
```

I checked the current directory using:

```bash
pwd
```

---

## Step 3: Create the Dockerfile

I created a file named exactly:

```text
Dockerfile
```

Using:

```bash
vi Dockerfile
```

or:

```bash
nano Dockerfile
```

The Dockerfile was then created according to the requirements given in the KodeKloud task.

A basic Dockerfile structure looks like:

```dockerfile
FROM <base-image>

# Additional instructions required by the task
```

---

## Step 4: Understand the Dockerfile

The most important instruction in a Dockerfile is usually:

```dockerfile
FROM <base-image>
```

The `FROM` instruction specifies the base image from which the new Docker image will be built.

For example:

```dockerfile
FROM ubuntu:latest
```

means that the Docker image will use Ubuntu as its base image.

---

## Step 5: Verify the Dockerfile

After creating the file, I verified that it existed:

```bash
ls -l Dockerfile
```

I also checked its contents:

```bash
cat Dockerfile
```

This allowed me to make sure that the Dockerfile contained the required instructions.

---

## Step 6: Understand the Docker Build Process

A Dockerfile is not itself a Docker image.

The general workflow is:

```text
Dockerfile
     |
     v
docker build
     |
     v
Docker Image
     |
     v
docker run
     |
     v
Docker Container
```

The Dockerfile provides the instructions, Docker uses those instructions to build an image, and the image can then be used to create containers.

---

# Common Dockerfile Instructions

## FROM

Defines the base image:

```dockerfile
FROM ubuntu:latest
```

---

## RUN

Executes a command while building the image:

```dockerfile
RUN apt-get update
```

---

## COPY

Copies files from the build context into the image:

```dockerfile
COPY index.html /app/
```

---

## WORKDIR

Sets the working directory:

```dockerfile
WORKDIR /app
```

---

## EXPOSE

Documents the port that the application uses:

```dockerfile
EXPOSE 8080
```

---

## CMD

Defines the default command when a container starts:

```dockerfile
CMD ["echo", "Hello Docker"]
```

---

# What I Learned

From this challenge, I learned and practiced:

- What a Dockerfile is.
- How Dockerfiles are used to build Docker images.
- How to create a file named `Dockerfile`.
- The purpose of the `FROM` instruction.
- The purpose of common Dockerfile instructions such as `RUN`, `COPY`, `WORKDIR`, `EXPOSE`, and `CMD`.
- The difference between a Dockerfile, Docker image, and Docker container.
- How to verify the Dockerfile using `ls` and `cat`.
- The basic Docker image-building workflow.

# Challenge Status

Day 41 — Completed Successfully 

**Another Docker challenge completed. Learning Dockerfiles one step at a time.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
