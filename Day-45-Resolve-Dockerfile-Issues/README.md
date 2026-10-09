# Day 45 - Resolve Dockerfile Issues

## Challenge Overview

This is Day 45 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Dockerfile Troubleshooting**. The task required identifying and resolving an issue in a Dockerfile caused by an incorrect path in its configuration.

The objective was to inspect the Dockerfile, identify the incorrect path, update the configuration, and verify that the issue had been resolved.

### Challenge Requirement

Resolve the Dockerfile issues by correcting the **incorrect path** used in the configuration.

The task involved inspecting the existing Dockerfile, identifying the path-related problem, correcting the configuration, and verifying the solution.

---

## Objectives

The objectives of this challenge are:

- Understand the purpose of a Dockerfile.
- Locate and inspect the existing Dockerfile.
- Identify an incorrect path in the configuration.
- Correct the path according to the task requirements.
- Verify the updated Dockerfile.
- Build the Docker image to check for errors, if required.
- Understand common Dockerfile troubleshooting techniques.
- Learn why correct file paths are important when building Docker images.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 45 |
| Topic | Dockerfile Troubleshooting |
| Issue | Incorrect Path |
| Configuration File | `Dockerfile` |
| Main Tool | Docker |
| Status | Completed |

---

# Solution

## Step 1: Access the Required Server

First, I accessed the server specified in the KodeKloud task using SSH.

```bash
ssh <username>@<server-hostname>
```

After connecting, I verified the server:

```bash
hostname
```

This helped me confirm that I was working on the correct server.

---

## Step 2: Locate the Dockerfile

Next, I navigated to the working directory specified in the task.

I checked the available files and directories:

```bash
pwd
ls -la
```

I located the Dockerfile that needed troubleshooting.

---

## Step 3: Inspect the Dockerfile

I examined the Dockerfile to understand its existing configuration:

```bash
cat Dockerfile
```

I carefully checked the instructions, especially those involving file and directory paths, such as:

```dockerfile
WORKDIR /app
COPY index.html /app/
```

The actual instructions depend on the Dockerfile provided in the challenge.

I identified that the problem was caused by an incorrect path in the configuration.

---

## Step 4: Edit the Dockerfile

I opened the Dockerfile for editing:

```bash
vi Dockerfile
```

I corrected the incorrect path according to the task requirements and saved the file.

When editing a Dockerfile, it is important to ensure that:

- The source file or directory exists in the build context.
- The destination path is correct.
- The `WORKDIR` instruction points to the intended working directory.
- File paths match the actual project structure.

---

## Step 5: Verify the Updated Dockerfile

After making the changes, I checked the Dockerfile again:

```bash
cat Dockerfile
```

I verified that the incorrect path had been corrected and that the configuration matched the task requirements.

---

## Step 6: Build the Docker Image

If the task requires a build test, I used:

```bash
docker build -t dockerfile-test .
```

This command attempts to build an image using the Dockerfile in the current directory.

A successful build indicates that Docker was able to process the Dockerfile and complete the build steps.

The image name `dockerfile-test` is an example for verification, not necessarily the image name required by the challenge.

---

## Step 7: Verify the Result

I reviewed the corrected configuration and performed the required verification.

Useful commands include:

```bash
cat Dockerfile
```

and, when appropriate:

```bash
docker images
```

The final check was to ensure that the path issue was resolved and the KodeKloud task requirements were satisfied.

---

# Understanding the Problem

A Dockerfile contains instructions that tell Docker how to build an image.

For example:

```dockerfile
FROM ubuntu:latest
WORKDIR /app
COPY index.html /app/
```

In this example:

- `FROM` selects the base image.
- `WORKDIR` sets the working directory inside the image.
- `COPY` copies a file from the build context into the image.

If a path is incorrect, Docker might not find the source file or might place the file in the wrong destination.

**Important:** The correct fix depends on the actual Dockerfile and the task requirements. Do not blindly replace paths with `/app` or any other directory.

---

# What I Learned

From this challenge, I learned and practiced:

- How Dockerfiles define image build instructions.
- How to inspect Dockerfile configuration.
- How to identify incorrect file and directory paths.
- How to edit and save a Dockerfile.
- How `WORKDIR` and `COPY` work.
- How to verify a corrected Dockerfile.
- How to build a Docker image for testing.
- Why the Docker build context matters.
- How to troubleshoot Docker configuration issues systematically.

# Challenge Status

Day 45 — Completed Successfully 

**One incorrect path, one troubleshooting lesson, and another step forward in my DevOps journey.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
