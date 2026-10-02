# Day 38 - Pull Docker Image and Change Its Tag

## Challenge Overview

This is Day 38 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Docker Images and Tags**. The task required pulling a Docker image from a registry and changing its tag according to the given requirement.

This challenge helped me understand how Docker images are downloaded, how image tags work, and how to create a new tag for an existing Docker image.

### Challenge Requirement

The task was to:

* Pull the required Docker image.
* Change the tag of the pulled image.
* Verify that the image has the required tag.

---

## Objectives

The objectives of this challenge are:

* Understand Docker images.
* Understand Docker image tags.
* Pull an image from a Docker registry.
* Create a new tag for an existing image.
* Verify Docker images and their tags.
* Understand the difference between an image and its tag.
* Practice basic Docker image management commands.

---

## Environment

| Item          | Details                                      |
| ------------- | -------------------------------------------- |
| Challenge     | 100 Days of DevOps                           |
| Platform      | KodeKloud                                    |
| Day           | 38                                           |
| Technology    | Docker                                       |
| Task          | Pull Image and Change Tag                    |
| Main Commands | `docker pull`, `docker tag`, `docker images` |
| Status        | Completed                                    |

---

# Solution

## Step 1: Access the Docker Server

First, I accessed the required server using SSH.

The SSH command format is:

```bash
ssh <username>@<server-hostname>
```

After connecting, I verified that Docker was available:

```bash
docker --version
```

---

## Step 2: Pull the Required Docker Image

I pulled the Docker image specified in the KodeKloud task.

The general command is:

```bash
docker pull <image>:<source-tag>
```

For example:

```bash
docker pull nginx:latest
```

Docker downloads the image from the configured container registry if it is not already available locally.

---

## Step 3: Verify the Pulled Image

After pulling the image, I checked the available Docker images:

```bash
docker images
```

The output displayed the image name, tag, image ID, creation time, and size.

---

## Step 4: Change the Image Tag

Next, I created the required new tag for the image.

The general syntax is:

```bash
docker tag <image>:<source-tag> <image>:<new-tag>
```

For example:

```bash
docker tag nginx:latest nginx:stable
```

This creates the `stable` tag for the existing `nginx` image.

---

## Step 5: Verify the New Tag

Finally, I verified the Docker image tags:

```bash
docker images
```

The output should show the required image with the new tag.

For example:

```text
REPOSITORY   TAG       IMAGE ID
nginx        latest    <image-id>
nginx        stable    <image-id>
```

The image ID can be the same because both tags can point to the same underlying image.

---

# Understanding Docker Tags

A Docker image is commonly written as:

```text
repository:tag
```

For example:

```text
nginx:latest
```

Here:

```text
nginx
```

is the image repository/name, and:

```text
latest
```

is the image tag.

Another example:

```text
nginx:stable
```

means the same repository with a different tag.

---

# What Does docker tag Do?

The command:

```bash
docker tag nginx:latest nginx:stable
```

does not rebuild or duplicate the image.

Instead, Docker creates another reference to the existing image.

Conceptually:

```text
              ┌── nginx:latest
Image ────────┤
              └── nginx:stable
```

Both tags can point to the same image ID.

---

# What I Learned

From this challenge, I learned and practiced:

* How to pull Docker images.
* How Docker image tags work.
* How to use `docker pull`.
* How to use `docker tag`.
* How to verify Docker images using `docker images`.
* The difference between an image and its tag.
* How multiple tags can reference the same Docker image.
* Basic Docker image management.
* Why image tags are useful for identifying different versions or releases.

# Challenge Status

Day 38 — Completed Successfully 

**Pulled the Docker image, changed its tag, and verified the new tag successfully.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
