# Day 40 - Docker Exec Operations

## Challenge Overview

This is Day 40 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Docker Exec Operations**. The objective was to access a running Docker container, install Apache2 inside the container, change the Apache listening port, start the Apache service, and verify the web server using `curl`.

This challenge helped me understand how to execute commands inside an already-running Docker container using `docker exec`.

### Challenge Requirement

Access the running Docker container and perform the following operations:

* Access the Docker container using `docker exec`.
* Install Apache2 inside the container.
* Change the Apache listening port.
* Start the Apache service.
* Verify that Apache is running.
* Test the Apache web server using `curl`.

---

## Objectives

The objectives of this challenge are:

* Understand the purpose of `docker exec`.
* List running Docker containers.
* Access a running container using an interactive shell.
* Install Apache2 inside a Docker container.
* Modify Apache configuration.
* Change the Apache listening port.
* Start the Apache service.
* Verify the Apache configuration.
* Test Apache using `curl`.
* Understand the difference between the Docker host and a Docker container.

---

## Environment

| Item              | Details                  |
| ----------------- | ------------------------ |
| Challenge         | 100 Days of DevOps       |
| Platform          | KodeKloud                |
| Day               | 40                       |
| Topic             | Docker Exec Operations   |
| Container         | Running Docker Container |
| Web Server        | Apache2                  |
| Original Port     | 80                       |
| New Port          | Required Task Port       |
| Verification Tool | `curl`                   |
| Status            | Completed                |

---

# Solution

## Step 1: Check Running Containers

First, I checked the currently running Docker containers:

```bash
docker ps
```

This command displays the running containers along with their container IDs, names, images, and port information.

I identified the container that needed to be modified.

---

## Step 2: Access the Running Container

I accessed the running container using `docker exec`:

```bash
docker exec -it <container-name> /bin/bash
```

For example:

```bash
docker exec -it ubuntu_latest /bin/bash
```

The `-it` options provide an interactive terminal.

After running the command, I was inside the Docker container.

---

## Step 3: Verify the Container

After entering the container, I verified that I was working inside the container:

```bash
hostname
```

I could also check the operating system:

```bash
cat /etc/os-release
```

---

## Step 4: Update Package Information

Since the container was Ubuntu-based, I updated the package information:

```bash
apt update
```

---

## Step 5: Install Apache2

I installed Apache2 inside the container:

```bash
apt install apache2 -y
```

This installed the Apache web server inside the Docker container.

---

## Step 6: Check Apache Configuration

Apache normally listens on port `80`.

I checked the Apache ports configuration:

```bash
cat /etc/apache2/ports.conf
```

The default configuration contains:

```text
Listen 80
```

I changed the listening port to the port required by the challenge.

For example:

```text
Listen 8080
```

I edited the file using:

```bash
nano /etc/apache2/ports.conf
```

---

## Step 7: Change the Virtual Host Port

I also updated the Apache default virtual host:

```bash
nano /etc/apache2/sites-enabled/000-default.conf
```

I changed:

```text
<VirtualHost *:80>
```

to the required port, for example:

```text
<VirtualHost *:8080>
```

This ensures that Apache's virtual host configuration matches the new listening port.

---

## Step 8: Test Apache Configuration

Before starting Apache, I checked the configuration:

```bash
apache2ctl configtest
```

Expected output:

```text
Syntax OK
```

This confirmed that there were no syntax errors in the Apache configuration.

---

## Step 9: Start Apache

I started Apache inside the container:

```bash
service apache2 start
```

I then checked its status:

```bash
service apache2 status
```

---

## Step 10: Verify the Listening Port

I checked which ports were listening:

```bash
ss -lntp
```

Apache should be listening on the newly configured port.

For example:

```text
LISTEN ... 0.0.0.0:8080
```

---

## Step 11: Verify Apache Using curl

Finally, I tested Apache using `curl`:

```bash
curl http://localhost:8080
```

If Apache was working correctly, the command returned the HTML content of the Apache default page.

This confirmed that Apache was successfully installed, configured, started, and listening on the new port.

---

# Understanding docker exec

The most important command in this challenge was:

```bash
docker exec -it <container-name> /bin/bash
```

`docker exec` allows us to execute commands inside a running container.

The command can be understood as:

```text
docker exec
     |
     └── Execute a command
          |
          └── -it
               |
               └── Interactive terminal
                    |
                    └── /bin/bash
                         |
                         └── Open Bash shell
```

Unlike `docker run`, `docker exec` does not create a new container.

It works with an **already-running container**.

---

# Troubleshooting

If the container is not running, first check:

```bash
docker ps -a
```

If the container is stopped, start it:

```bash
docker start <container-name>
```

Then access it:

```bash
docker exec -it <container-name> /bin/bash
```

If `curl` is not installed inside the container, it can be installed using:

```bash
apt update
apt install curl -y
```

If Apache configuration has an error, check:

```bash
apache2ctl configtest
```

If Apache does not start, check:

```bash
service apache2 status
```

---

# What I Learned

From this challenge, I learned and practiced:

* How to list running Docker containers using `docker ps`.
* How to access a running container using `docker exec`.
* How to work inside a Docker container.
* How to install Apache2 inside an Ubuntu container.
* How to modify Apache configuration files.
* How to change the Apache listening port.
* How to start and check the Apache service.
* How to check listening ports using `ss`.
* How to validate Apache configuration using `apache2ctl configtest`.
* How to test a web server using `curl`.
* The difference between the Docker host and a container.

# Challenge Status

Day 40 — Completed Successfully 

**Accessed a running Docker container, installed Apache2, changed its listening port, and verified the web server using `curl`.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
