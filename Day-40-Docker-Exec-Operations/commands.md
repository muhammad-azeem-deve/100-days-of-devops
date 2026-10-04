# Day 40 - Commands

This file contains the commands used during **Day 40 of my 100 Days of DevOps Challenge**.

The objective was to access a running Docker container, install Apache2, change its listening port, and verify Apache using `curl`.

---

# Step 1: List Running Containers

```bash
docker ps
```

This displays all currently running Docker containers.

---

# Step 2: Access the Docker Container

```bash
docker exec -it <container-name> /bin/bash
```

Example:

```bash
docker exec -it ubuntu_latest /bin/bash
```

---

# Step 3: Verify the Container

```bash
hostname
```

Check the operating system:

```bash
cat /etc/os-release
```

---

# Step 4: Update Package Information

Inside the container:

```bash
apt update
```

---

# Step 5: Install Apache2

```bash
apt install apache2 -y
```

---

# Step 6: Check Apache Port Configuration

```bash
cat /etc/apache2/ports.conf
```

The default configuration normally contains:

```text
Listen 80
```

---

# Step 7: Change Apache Listening Port

Open the Apache ports configuration:

```bash
nano /etc/apache2/ports.conf
```

Change:

```text
Listen 80
```

to the required port.

Example:

```text
Listen 8080
```

---

# Step 8: Update Apache Virtual Host

Open the default virtual host configuration:

```bash
nano /etc/apache2/sites-enabled/000-default.conf
```

Change:

```text
<VirtualHost *:80>
```

to:

```text
<VirtualHost *:8080>
```

Use the actual port specified by the KodeKloud task.

---

# Step 9: Test Apache Configuration

```bash
apache2ctl configtest
```

Expected output:

```text
Syntax OK
```

---

# Step 10: Start Apache

```bash
service apache2 start
```

---

# Step 11: Check Apache Status

```bash
service apache2 status
```

---

# Step 12: Check Listening Ports

```bash
ss -lntp
```

Verify that Apache is listening on the newly configured port.

---

# Step 13: Install curl if Required

If `curl` is not available inside the container:

```bash
apt update
apt install curl -y
```

---

# Step 14: Test Apache Using curl

Use the new Apache port:

```bash
curl http://localhost:8080
```

Replace `8080` with the actual port required by the challenge.

A successful response should contain HTML from the Apache default page.

---

# Quick Command Summary

```bash
# List running containers
docker ps

# Access container
docker exec -it <container-name> /bin/bash

# Verify container
hostname

# Update packages
apt update

# Install Apache2
apt install apache2 -y

# Check Apache port configuration
cat /etc/apache2/ports.conf

# Edit Apache port
nano /etc/apache2/ports.conf

# Edit Apache virtual host
nano /etc/apache2/sites-enabled/000-default.conf

# Test Apache configuration
apache2ctl configtest

# Start Apache
service apache2 start

# Check Apache status
service apache2 status

# Check listening ports
ss -lntp

# Test Apache
curl http://localhost:<new-port>
```

---

# Important Commands Explained

## docker exec

```bash
docker exec -it <container-name> /bin/bash
```

Used to open an interactive Bash shell inside a running container.

---

## Apache Configuration

```text
/etc/apache2/ports.conf
```

Controls the ports on which Apache listens.

---

## Apache Virtual Host

```text
/etc/apache2/sites-enabled/000-default.conf
```

Contains the default Apache virtual host configuration.

---

## Configuration Test

```bash
apache2ctl configtest
```

Checks Apache configuration for syntax errors.

Expected result:

```text
Syntax OK
```

---

## Check Listening Ports

```bash
ss -lntp
```

Shows TCP ports currently listening inside the container.

---

## Verify with curl

```bash
curl http://localhost:<new-port>
```

Sends an HTTP request to Apache and displays the response.

---

# Important Lesson

The key concept in this challenge was **`docker exec`**.

The command:

```bash
docker exec -it <container-name> /bin/bash
```

allows us to work directly inside an already-running Docker container.

The overall process was:

```text
Running Container
       ↓
docker exec
       ↓
Enter Container
       ↓
Install Apache2
       ↓
Change Apache Port
       ↓
Start Apache
       ↓
Check Listening Port
       ↓
curl localhost:<port>
       ↓
Apache Response
```

**Day 40 completed successfully.**
