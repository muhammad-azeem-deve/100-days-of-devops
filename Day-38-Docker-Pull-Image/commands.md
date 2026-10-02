# Day 38 - Commands

This file contains the commands used during **Day 38 of my 100 Days of DevOps Challenge**.

The objective was to pull the required Docker image and change its tag.

---

# Step 1: Access the Server

```bash
ssh <username>@<server-hostname>
```

---

# Step 2: Verify Docker

```bash
docker --version
```

---

# Step 3: Pull the Required Image

General syntax:

```bash
docker pull <image>:<source-tag>
```

Example:

```bash
docker pull nginx:latest
```

---

# Step 4: Verify the Image

```bash
docker images
```

---

# Step 5: Change the Image Tag

General syntax:

```bash
docker tag <image>:<source-tag> <image>:<new-tag>
```

Example:

```bash
docker tag nginx:latest nginx:stable
```

---

# Step 6: Verify the New Tag

```bash
docker images
```

Example output:

```text
REPOSITORY   TAG       IMAGE ID
nginx        latest    <image-id>
nginx        stable    <image-id>
```

---

# Quick Command Summary

```bash
# Check Docker
docker --version

# Pull image
docker pull <image>:<source-tag>

# Check images
docker images

# Create a new tag
docker tag <image>:<source-tag> <image>:<new-tag>

# Verify the new tag
docker images
```

---

# Example

If the required image is:

```text
nginx:latest
```

and the new tag is:

```text
nginx:stable
```

use:

```bash
docker pull nginx:latest
docker tag nginx:latest nginx:stable
docker images
```

---

# Important Lesson

The `docker tag` command does not create a new image from scratch.

It creates a new tag/reference for an existing Docker image.

The basic workflow is:

```text
docker pull
     ↓
Pull Image
     ↓
docker tag
     ↓
Create New Tag
     ↓
docker images
     ↓
Verify
```

The most important commands for this challenge are:

```bash
docker pull <image>:<source-tag>
docker tag <image>:<source-tag> <image>:<new-tag>
docker images
```

**Day 38 completed successfully.** 
