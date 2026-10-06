# Day 41 - Commands

This file contains the commands used during **Day 41 of my 100 Days of DevOps Challenge**.

The objective was to create a **Dockerfile** according to the requirements of the KodeKloud challenge.

---

# Step 1: Access the Server

Connect to the required server:

```bash
ssh <username>@<server-hostname>
```

---

# Step 2: Verify the Server

Check the hostname:

```bash
hostname
```

---

# Step 3: Navigate to the Required Directory

```bash
cd <required-directory>
```

Check the current directory:

```bash
pwd
```

---

# Step 4: Create the Dockerfile

Create a file named:

```text
Dockerfile
```

Using `vi`:

```bash
vi Dockerfile
```

Or using `nano`:

```bash
nano Dockerfile
```

---

# Step 5: Verify the Dockerfile

Check that the file exists:

```bash
ls -l Dockerfile
```

---

# Step 6: Display the Dockerfile

View its contents:

```bash
cat Dockerfile
```

---

# Step 7: Check Dockerfile Syntax

Docker does not provide a separate universal `dockerfile validate` command. A practical way to validate the Dockerfile is to build the image:

```bash
docker build -t test-image .
```

If the build completes successfully, Docker was able to process the Dockerfile.

---

# Quick Command Summary

```bash
# Connect to server
ssh <username>@<server-hostname>

# Verify server
hostname

# Navigate to required directory
cd <required-directory>

# Check current directory
pwd

# Create Dockerfile
vi Dockerfile

# Verify Dockerfile
ls -l Dockerfile

# View Dockerfile
cat Dockerfile

# Build image to test Dockerfile
docker build -t test-image .
```

---

# Dockerfile Example Structure

A basic Dockerfile can look like:

```dockerfile
FROM <base-image>

# Additional instructions
```

For example:

```dockerfile
FROM ubuntu:latest
```

The exact contents should always follow the requirements of the KodeKloud task.

---

# Important Dockerfile Instructions

```dockerfile
FROM
```

Specifies the base image.

```dockerfile
RUN
```

Runs commands while building the image.

```dockerfile
COPY
```

Copies files into the image.

```dockerfile
WORKDIR
```

Sets the working directory.

```dockerfile
EXPOSE
```

Documents the container port.

```dockerfile
CMD
```

Specifies the default command when the container starts.

---

# Important Lesson

A **Dockerfile is a blueprint for a Docker image**.

The basic workflow is:

```text
Dockerfile
     ↓
docker build
     ↓
Docker Image
     ↓
docker run
     ↓
Docker Container
```

The most important thing in this challenge was to create the `Dockerfile` in the **correct location** and follow the exact requirements provided by KodeKloud.

**Day 41 completed successfully.** 
