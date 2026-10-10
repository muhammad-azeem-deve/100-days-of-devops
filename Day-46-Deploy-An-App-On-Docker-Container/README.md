# Day 46 - Deploy an App Using Docker Compose

## Challenge Overview

This is Day 46 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Docker Compose and Container Deployment**. The task required deploying an application using a Docker Compose file containing two services: `web` and `db`.

The web service depended on the database service. Port binding and volumes were also configured for both services.

### Challenge Requirement

Deploy an application using Docker Compose with the following requirements:

- Create two services named `web` and `db`.
- Configure the web service to depend on the database service.
- Configure port binding for the services as required.
- Add volumes to both services.
- Start the services using Docker Compose.
- Verify that the containers are running successfully.

---

## Objectives

The objectives of this challenge are:

- Understand Docker Compose.
- Create a Docker Compose YAML file.
- Deploy multiple containers using one configuration file.
- Configure service dependencies using `depends_on`.
- Configure port binding using `ports`.
- Configure persistent storage using `volumes`.
- Start services in detached mode.
- Verify running containers and inspect logs.
- Understand how Docker Compose simplifies container management.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 46 |
| Technology | Docker Compose |
| Services | `web`, `db` |
| Dependency | Web depends on DB |
| Port Binding | Configured as required |
| Volumes | Configured for both services |
| Configuration File | `docker-compose.yml` |
| Status | Completed |

---

# Solution

## Step 1: Verify Docker Installation

First, I checked whether Docker was installed on the server.

```bash
docker --version
```

I also checked Docker Compose:

```bash
docker-compose --version
```

Depending on the installed version, the Compose command may instead be:

```bash
docker compose version
```

---

## Step 2: Create the Docker Compose File

I created or opened the Compose configuration file:

```bash
nano docker-compose.yml
```

The file defines the application services, their dependencies, port mappings, and volumes.

---

## Step 3: Configure the Web and Database Services

The Compose file contained two services:

- `web`: Runs the web application.
- `db`: Runs the database service.

The `web` service was configured with:

```yaml
depends_on:
  - db
```

This tells Docker Compose to start the database service before starting the web service.

**Note:** `depends_on` controls startup order, but does not by itself guarantee that the database is ready to accept connections.

---

## Step 4: Configure Port Binding

Port binding was configured to allow access to the required container ports.

The general format is:

```yaml
ports:
  - "HOST_PORT:CONTAINER_PORT"
```

For example:

```yaml
ports:
  - "8080:80"
```

This maps host port `8080` to container port `80`.

The actual port numbers must match the requirements of the application and the KodeKloud task.

---

## Step 5: Configure Volumes

Volumes were configured for both services.

Volumes provide storage that can exist independently of a container's lifecycle.

For example, a database service may use:

```yaml
volumes:
  - db_data:/var/lib/mysql
```

This mounts a named volume at the database's data directory.

A web service can use a bind mount such as:

```yaml
volumes:
  - ./web:/usr/share/nginx/html
```

This example maps a local directory into the container. The correct mount paths depend on the selected image.

---

## Step 6: Validate the Compose Configuration

Before starting the services, I checked the Compose file for configuration errors.

```bash
docker-compose config
```

For the newer Docker Compose plugin, I used:

```bash
docker compose config
```

This helps identify YAML syntax errors and invalid Compose settings.

---

## Step 7: Deploy the Application

I started the services using Docker Compose:

```bash
docker-compose up -d
```

Alternatively:

```bash
docker compose up -d
```

The `up` command creates and starts the services, while `-d` runs them in the background.

Docker Compose manages both services using the same configuration file.

---

## Step 8: Verify the Running Containers

I verified the services using:

```bash
docker-compose ps
```

Or:

```bash
docker compose ps
```

I checked the logs to identify any application or startup errors:

```bash
docker-compose logs
```

To inspect the web service separately:

```bash
docker-compose logs web
```

---

## Step 9: Test the Application

If the web service was exposed on host port `8080`, I could test it using:

```bash
curl http://localhost:8080
```

The exact URL and port depend on the actual port binding configured in the Compose file.

---

# Understanding Docker Compose

Docker Compose allows multiple related containers to be defined and managed through a single YAML file.

Instead of manually creating and starting each container, I can define the services once and manage them together.

### Important Compose Options

| Option | Purpose |
|--------|---------|
| `services` | Defines the containers and their configurations |
| `image` | Specifies the container image |
| `depends_on` | Defines service startup dependencies |
| `ports` | Maps host ports to container ports |
| `volumes` | Mounts persistent storage or directories |
| `environment` | Sets environment variables |
| `up -d` | Creates and starts services in the background |
| `ps` | Lists Compose services and their status |
| `logs` | Displays service logs |
| `down` | Stops and removes Compose containers and network |

---

# What I Learned

From this challenge, I learned and practiced:

- How Docker Compose simplifies multi-container deployments.
- How to define web and database services in a YAML file.
- How to configure dependencies between services.
- How port binding exposes container services.
- How volumes provide storage for containers.
- How to validate a Compose configuration.
- How to deploy multiple services using one command.
- How to verify containers using Docker Compose.
- How to troubleshoot startup problems through container logs.
- How to stop a Compose deployment without automatically deleting named volumes.

# Challenge Status

Day 46 — Completed Successfully

**Two services, one Compose file, and a better understanding of multi-container deployment.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
