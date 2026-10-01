# Day 37 - Commands

This file contains the commands used during **Day 37 of my 100 Days of DevOps Challenge**.

The objective was to copy `/tmp/nautilus.txt.gpg` from App Server 2 into the `/tmp/` directory of the `ubuntu_latest` Docker container without modifying the file.

---

# Step 1: Access App Server 2

```bash
ssh <username>@<server-hostname>
```

Example:

```bash
ssh tony@stapp02
```

---

# Step 2: Verify the Server

```bash
hostname
```

---

# Step 3: Check Running Containers

```bash
sudo docker ps
```

Verify that the following container is running:

```text
ubuntu_latest
```

---

# Step 4: Verify the Source File

```bash
ls -l /tmp/nautilus.txt.gpg
```

---

# Step 5: Copy the File to the Container

```bash
sudo docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/tmp/
```

---

# Step 6: Verify the File Inside the Container

```bash
sudo docker exec ubuntu_latest ls -l /tmp/nautilus.txt.gpg
```

---

# Step 7: Verify File Integrity on the Host

```bash
sha256sum /tmp/nautilus.txt.gpg
```

---

# Step 8: Verify File Integrity Inside the Container

```bash
sudo docker exec ubuntu_latest sha256sum /tmp/nautilus.txt.gpg
```

The SHA-256 checksum should match the checksum calculated on the Docker host.

---

# Quick Command Summary

```bash
# Access App Server 2
ssh <username>@<server-hostname>

# Verify server
hostname

# Check running containers
sudo docker ps

# Verify source file
ls -l /tmp/nautilus.txt.gpg

# Copy file to container
sudo docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/tmp/

# Verify file inside container
sudo docker exec ubuntu_latest ls -l /tmp/nautilus.txt.gpg

# Check host checksum
sha256sum /tmp/nautilus.txt.gpg

# Check container checksum
sudo docker exec ubuntu_latest sha256sum /tmp/nautilus.txt.gpg
```

---

# Important Docker Command

The main command for this challenge was:

```bash
sudo docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/tmp/
```

Its structure is:

```text
docker cp <source> <container>:<destination>
```

For this challenge:

```text
Source:
 /tmp/nautilus.txt.gpg

Container:
 ubuntu_latest

Destination:
 /tmp/
```

---

# Important Lesson

The file was an encrypted `.gpg` file, so it was **not necessary to decrypt or edit it**.

The correct approach was simply to copy the file:

```bash
sudo docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/tmp/
```

Then verify that the file exists inside the container and compare the SHA-256 checksums to confirm that its contents were not modified.

**Day 37 completed successfully.** 
