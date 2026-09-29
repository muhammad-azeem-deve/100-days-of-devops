# Day 35 - Commands

This file contains the commands used during **Day 35 of my 100 Days of DevOps Challenge**.

The objective was to install **Docker CE** and **Docker Compose** on **Application Server 3** and start the Docker service.

---

# Step 1: Access Application Server 3

Connect to App Server 3:

```bash
ssh <username>@<server3-hostname>
```

Example:

```bash
ssh <username>@stapp03
```

---

# Step 2: Verify the Server

Check the hostname:

```bash
hostname
```

---

# Step 3: Install Docker CE

Install Docker CE:

```bash
sudo yum install docker-ce -y
```

---

# Step 4: Install Docker Compose

Install Docker Compose:

```bash
sudo yum install docker-compose -y
```

---

# Step 5: Start Docker Service

Start the Docker service:

```bash
sudo systemctl start docker
```

---

# Step 6: Check Docker Service

Verify that Docker is running:

```bash
sudo systemctl status docker
```

Expected state:

```text
active (running)
```

---

# Step 7: Verify Docker Installation

Check the Docker version:

```bash
docker --version
```

---

# Step 8: Verify Docker Compose

Check Docker Compose:

```bash
docker-compose --version
```

If the installed version uses the newer Compose plugin, use:

```bash
docker compose version
```

---

# Quick Command Summary

```bash
# Access App Server 3
ssh <username>@stapp03

# Verify server
hostname

# Install Docker CE
sudo yum install docker-ce -y

# Install Docker Compose
sudo yum install docker-compose -y

# Start Docker
sudo systemctl start docker

# Check Docker service
sudo systemctl status docker

# Verify Docker
docker --version

# Verify Docker Compose
docker-compose --version
```

---

# Important Verification

The most important checks for this challenge are:

```bash
docker --version
```

```bash
docker-compose --version
```

and:

```bash
sudo systemctl status docker
```

The Docker service should show:

```text
active (running)
```

---

# Important Lesson

Installing Docker and starting Docker are two different steps.

First, install Docker:

```bash
sudo yum install docker-ce -y
```

Then start the Docker service:

```bash
sudo systemctl start docker
```

Finally, verify it:

```bash
sudo systemctl status docker
```

For this challenge, the main goal was simply to prepare **App Server 3** for future containerization work.

**Day 35 completed successfully.** 
