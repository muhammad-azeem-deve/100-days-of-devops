# Day 45 - Commands

This file contains the commands used during **Day 45 of my 100 Days of DevOps Challenge by KodeKloud**.

The objective was to resolve a Dockerfile issue caused by an incorrect path in the configuration.

---

## Step 1: Access the Server

```bash
ssh <username>@<server-hostname>
```

---

## Step 2: Verify the Server

```bash
hostname
```

---

## Step 3: Check the Current Directory

```bash
pwd
```

List the files and directories:

```bash
ls -la
```

---

## Step 4: Locate and Inspect the Dockerfile

Display the Dockerfile:

```bash
cat Dockerfile
```

If the Dockerfile is in another directory, navigate to that directory first.

---

## Step 5: Edit the Dockerfile

Open the Dockerfile:

```bash
vi Dockerfile
```

Correct the incorrect path according to the task requirements.

Save and exit the editor.

---

## Step 6: Verify the Changes

Display the updated Dockerfile:

```bash
cat Dockerfile
```

Check the relevant file paths and instructions.

---

## Step 7: Build the Docker Image

If required, build the image to test the Dockerfile:

```bash
docker build -t dockerfile-test .
```

The final `.` specifies the current directory as the build context.

---

## Step 8: Verify Docker Images

List the available Docker images:

```bash
docker images
```

If the build succeeds, the test image should appear in the output.

---

# Quick Command Summary

```bash
# Access the server
ssh <username>@<server-hostname>

# Verify the server
hostname

# Check the current directory
pwd

# List files and directories
ls -la

# Inspect the Dockerfile
cat Dockerfile

# Edit the Dockerfile
vi Dockerfile

# Verify the corrected configuration
cat Dockerfile

# Build the image if required
docker build -t dockerfile-test .

# List Docker images
docker images
```

---

# Important Dockerfile Instructions

```dockerfile
FROM ubuntu:latest
WORKDIR /app
COPY index.html /app/
```

- `FROM` specifies the base image.
- `WORKDIR` specifies the working directory inside the image.
- `COPY` copies files from the build context into the image.

---

# Important Lesson

When troubleshooting a Dockerfile, do not change random configuration lines.

Follow this process:

1. Inspect the existing Dockerfile.
2. Identify the incorrect path.
3. Check the actual project files and directories.
4. Correct only what is necessary.
5. Review the updated Dockerfile.
6. Build and verify the image when appropriate.
7. Confirm that the original task requirements are satisfied.

**The main lesson from Day 45 is that even a small path mistake can prevent a Docker image from building correctly or cause files to be placed in the wrong location.**
