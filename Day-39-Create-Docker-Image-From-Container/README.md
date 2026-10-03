# Day 39 - Create a Docker Image from a Docker Container

## Challenge Overview

This is Day 39 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Docker Images and Containers**. The task was to create a new Docker image from an existing Docker container.

The main concept of this challenge was to understand how the current state of a Docker container can be saved as a new Docker image using the `docker commit` command.

### Challenge Requirement

Create a new Docker image from an existing Docker container.

The objective was to identify the required Docker container, create an image from that container, and verify that the new Docker image was created successfully.

---

## Objectives

The objectives of this challenge are:

* Understand the relationship between Docker containers and images.
* List existing Docker containers.
* Identify the required container.
* Create a Docker image from a container.
* Understand the `docker commit` command.
* Verify that the new Docker image exists.
* Understand how a container's current filesystem state can be saved as an image.
* Learn how to create a new container from the created image.

---

## Environment

| Item         | Details                            |
| ------------ | ---------------------------------- |
| Challenge    | 100 Days of DevOps                 |
| Platform     | KodeKloud                          |
| Day          | 39                                 |
| Technology   | Docker                             |
| Task         | Create Docker Image from Container |
| Main Command | `docker commit`                    |
| Status       | Completed                          |

---

# Solution

## Step 1: Check Existing Docker Containers

First, I checked the Docker containers available on the server.

I used:

```bash
docker ps -a
```

The `-a` option displays both running and stopped containers.

From the output, I identified the container that was required for the challenge.

---

## Step 2: Check the Container

After identifying the container, I checked its details using:

```bash
docker inspect <container-name>
```

This command provides detailed information about the container, including its image, configuration, networking, mounts, and other settings.

---

## Step 3: Create an Image from the Container

The main step of this challenge was creating a Docker image from the existing container.

I used:

```bash
docker commit <container-name> <new-image-name>
```

For example:

```bash
docker commit <container-name> my-custom-image
```

The `docker commit` command creates a new image from the current state of the container.

The general syntax is:

```bash
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
```

---

## Step 4: Verify the New Docker Image

After creating the image, I verified that it was successfully created.

I used:

```bash
docker images
```

The newly created image should appear in the list.

I could also use:

```bash
docker image ls
```

Both commands display the Docker images available on the system.

---

## Step 5: Check the Created Image

To get more information about the newly created image, I used:

```bash
docker inspect <image-name>
```

For example:

```bash
docker inspect my-custom-image
```

This displays detailed information about the image.

---

## Step 6: Run a Container from the New Image

To verify that the newly created image can be used to create a container, I can run:

```bash
docker run -it <image-name>
```

For example:

```bash
docker run -it my-custom-image
```

This creates a new container using the image created from the original container.

---

# Understanding the Concept

Normally, Docker follows this process:

```text
Dockerfile
    ↓
Docker Image
    ↓
Docker Container
```

In this challenge, we worked in the opposite direction:

```text
Existing Docker Container
          ↓
    docker commit
          ↓
     New Docker Image
```

The important command is:

```bash
docker commit <container-name> <image-name>
```

---

# Understanding docker commit

The command:

```bash
docker commit my-container my-image
```

means:

```text
Take the current state of my-container
              ↓
        Save it as an image
              ↓
          my-image
```

The new image can then be used to create additional containers.

---

# Important Note

`docker commit` is useful for learning, troubleshooting, and capturing the current state of a container.

However, for production applications, a **Dockerfile** is generally preferred because it makes image creation reproducible and easier to maintain.

For this KodeKloud challenge, the purpose was specifically to practice creating an image from an existing container using `docker commit`.

---

# What I Learned

From this challenge, I learned and practiced:

* The difference between Docker images and containers.
* How to list Docker containers using `docker ps -a`.
* How to inspect a Docker container.
* How to create a Docker image from a container.
* How to use the `docker commit` command.
* How to verify Docker images using `docker images`.
* How to inspect a Docker image.
* How to create a new container from a Docker image.
* How Docker can save the current state of a container as a new image.
* Why Dockerfiles are generally preferred for reproducible image builds.

# Challenge Status

Day 39 — Completed Successfully

**Created a Docker image from an existing Docker container and verified the new image successfully.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
