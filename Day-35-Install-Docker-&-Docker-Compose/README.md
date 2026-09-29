# Day 35 - Install Docker and Docker Compose

## Challenge Overview

This is Day 35 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Docker Installation and Service Management**. The Nautilus DevOps team wanted to start containerizing applications, so the first step was to prepare **Application Server 3** with Docker.

### Challenge Requirement

Install **Docker CE** and **Docker Compose** on **Application Server 3** and start the Docker service.

The objective was to access App Server 3, install the required Docker packages, start the Docker service, and verify that everything was working correctly.

---

## Objectives

The objectives of this challenge are:

* Access Application Server 3 using SSH.
* Verify that the correct server has been accessed.
* Install Docker CE.
* Install Docker Compose.
* Start the Docker service.
* Verify that Docker is running successfully.
* Verify the installed Docker and Docker Compose versions.
* Understand basic Docker service management using `systemctl`.

---

## Environment

| Item               | Details              |
| ------------------ | -------------------- |
| Challenge          | 100 Days of DevOps   |
| Platform           | KodeKloud            |
| Day                | 35                   |
| Server             | Application Server 3 |
| Docker Package     | Docker CE            |
| Additional Package | Docker Compose       |
| Service            | Docker               |
| Required State     | Active / Running     |
| Status             | Completed            |

---

# Solution

## Step 1: Access Application Server 3

First, I accessed **Application Server 3** using SSH.

The SSH command format is:

```bash
ssh <username>@<server3-hostname>
```

Example:

```bash
ssh <username>@stapp03
```

---

## Step 2: Verify the Server

After connecting to the server, I verified that I was working on the correct server:

```bash
hostname
```

This is important because the task specifically required Docker to be installed on **App Server 3**.

---

## Step 3: Install Docker CE

Next, I installed Docker CE using the package manager:

```bash
sudo yum install docker-ce -y
```

The `-y` option automatically confirms the installation prompts.

Docker CE provides the Docker Engine required to create and manage containers.

---

## Step 4: Install Docker Compose

After installing Docker CE, I installed Docker Compose:

```bash
sudo yum install docker-compose -y
```

Docker Compose is used to define and manage applications that contain multiple Docker containers.

---

## Step 5: Start the Docker Service

After installing Docker, I started the Docker service:

```bash
sudo systemctl start docker
```

This starts the Docker daemon so that Docker commands can communicate with the Docker engine.

---

## Step 6: Verify Docker Service

I checked the Docker service status:

```bash
sudo systemctl status docker
```

The expected state is:

```text
active (running)
```

This confirms that the Docker service is running successfully.

---

## Step 7: Verify Docker Installation

I checked the installed Docker version:

```bash
docker --version
```

This confirmed that Docker was successfully installed.

---

## Step 8: Verify Docker Compose Installation

I verified Docker Compose using:

```bash
docker-compose --version
```

Depending on the Docker Compose version installed on the server, the command may also be:

```bash
docker compose version
```

This confirms that Docker Compose is available.

---

# What I Learned

From this challenge, I learned and practiced:

* How to access a Linux server using SSH.
* How to install Docker CE using a package manager.
* How to install Docker Compose.
* How to start the Docker service using `systemctl`.
* How to check the status of a Linux service.
* How to verify Docker installation using `docker --version`.
* How to verify Docker Compose installation.
* The difference between installing Docker and starting the Docker daemon.
* Why the Docker service must be running before working with containers.

# Challenge Status

Day 35 — Completed Successfully 

**Docker environment prepared successfully on Application Server 3.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
