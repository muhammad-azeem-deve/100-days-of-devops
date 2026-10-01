# Day 37 - Copy Encrypted File to Docker Container

## Challenge Overview

This is Day 37 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Docker Container File Management** and required copying an encrypted file from the Docker host into a running Docker container.

The task was performed on **App Server 2** in the Stratos Datacenter.

### Challenge Requirement

Copy the following encrypted file:

```text
/tmp/nautilus.txt.gpg
```

from the Docker host into the following running container:

```text
ubuntu_latest
```

The file must be copied to:

```text
/tmp/
```

The file must **not be modified** during the operation.

---

## Objectives

The objectives of this challenge are:

* Access App Server 2 using SSH.
* Verify the Docker host.
* Check the running Docker containers.
* Identify the `ubuntu_latest` container.
* Verify that the source file exists on the Docker host.
* Copy the encrypted file into the container.
* Keep the original file unchanged.
* Verify that the file exists inside the container.
* Compare file checksums to confirm that the file was not modified.
* Understand how `docker cp` works.

---

## Environment

| Item                  | Details                 |
| --------------------- | ----------------------- |
| Challenge             | 100 Days of DevOps      |
| Platform              | KodeKloud               |
| Day                   | 37                      |
| Server                | App Server 2            |
| Container             | `ubuntu_latest`         |
| Source File           | `/tmp/nautilus.txt.gpg` |
| Container Destination | `/tmp/`                 |
| File Type             | Encrypted `.gpg` file   |
| Operation             | Docker file copy        |
| Status                | Completed               |

---

# Solution

## Step 1: Access App Server 2

First, I accessed **App Server 2** using SSH.

The SSH command format is:

```bash
ssh <username>@<server-hostname>
```

Example:

```bash
ssh tony@stapp02
```

---

## Step 2: Verify the Server

After connecting to the server, I verified the hostname:

```bash
hostname
```

This confirmed that I was working on the required server.

---

## Step 3: Check the Running Docker Containers

Next, I checked the running Docker containers:

```bash
sudo docker ps
```

I verified that the required container:

```text
ubuntu_latest
```

was running.

---

## Step 4: Verify the Source File

Before copying the file, I checked that the source file existed on the Docker host:

```bash
ls -l /tmp/nautilus.txt.gpg
```

The required source file was:

```text
/tmp/nautilus.txt.gpg
```

---

## Step 5: Copy the File into the Container

I used the Docker `cp` command to copy the encrypted file from the host into the container:

```bash
sudo docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/tmp/
```

The command copies the file without decrypting or modifying its contents.

The general syntax is:

```text
docker cp <source> <container>:<destination>
```

In this task:

```text
Source:
 /tmp/nautilus.txt.gpg

Container:
 ubuntu_latest

Destination:
 /tmp/
```

---

## Step 6: Verify the File Inside the Container

After copying the file, I verified that it existed inside the container:

```bash
sudo docker exec ubuntu_latest ls -l /tmp/nautilus.txt.gpg
```

This confirmed that the file was successfully copied to the required location.

---

## Step 7: Verify That the File Was Not Modified

Because the task specifically required that the file must not be modified, I compared the SHA-256 checksum of the original file with the copied file.

On the Docker host:

```bash
sha256sum /tmp/nautilus.txt.gpg
```

Inside the container:

```bash
sudo docker exec ubuntu_latest sha256sum /tmp/nautilus.txt.gpg
```

The two SHA-256 hashes should be identical.

For example:

```text
Host:
abc123...xyz

Container:
abc123...xyz
```

Matching checksums confirm that the file contents remained unchanged during the copy operation.

---

# Understanding the Docker Command

The main command used in this challenge was:

```bash
sudo docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/tmp/
```

The command can be divided into:

```text
docker cp
```

Copies files or directories between the Docker host and a container.

```text
/tmp/nautilus.txt.gpg
```

The source file on the Docker host.

```text
ubuntu_latest
```

The destination container.

```text
:/tmp/
```

The destination directory inside the container.

---

# What I Learned

From this challenge, I learned and practiced:

* How to access a Docker host using SSH.
* How to list running Docker containers using `docker ps`.
* How to copy files from a Docker host into a container.
* How to use the `docker cp` command.
* How to use `docker exec` to run commands inside a container.
* How to verify files inside a Docker container.
* How SHA-256 checksums can be used to verify file integrity.
* Why an encrypted file should be copied without opening or modifying it.
* The difference between the Docker host filesystem and the container filesystem.

# Challenge Status

Day 37 — Completed Successfully 

**Encrypted file copied successfully from App Server 2 into the `ubuntu_latest` container without modifying its contents.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
