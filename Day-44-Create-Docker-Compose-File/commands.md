# Day 44 - Commands

This file contains the commands used during **Day 44 of my 100 Days of DevOps Challenge**.

The objective was to create a Docker Compose file using the `httpd:latest` image and map host port `8087` to container port `80`.

---

# Step 1: Check Docker

Verify that Docker is installed:

```bash
docker --version
```

---

# Step 2: Check Docker Compose

Verify Docker Compose:

```bash
docker compose version
```

If the system uses the older Docker Compose command:

```bash
docker-compose --version
```

---

# Step 3: Create the Docker Compose File

Create:

```bash
nano docker-compose.yml
```

Add:

```yaml
services:
  web:
    image: httpd:latest
    ports:
      - "8087:80"
```

Save and exit.

---

# Step 4: Start the Container

Start the Compose application in detached mode:

```bash
docker compose up -d
```

---

# Step 5: Verify Running Containers

Check the running containers:

```bash
docker ps
```

Look for:

```text
0.0.0.0:8087->80/tcp
```

---

# Step 6: Verify Docker Compose Service

```bash
docker compose ps
```

---

# Step 7: Test Apache

Test Apache from the server:

```bash
curl http://localhost:8087
```

---


# Quick Command Summary

```bash
# Check Docker
docker --version

# Check Docker Compose
docker compose version

# Create Compose file
nano docker-compose.yml

# Start container
docker compose up -d

# Check running containers
docker ps

# Check Compose services
docker compose ps

# Test Apache
curl http://localhost:8087

# Check logs
docker compose logs

# Stop and remove containers
docker compose down
```

---

# Docker Compose File

The final `docker-compose.yml` was:

```yaml
services:
  web:
    image: httpd:latest
    ports:
      - "8087:80"
```

---

# Important Lesson

The most important concept in this challenge is Docker port mapping.

The format is:

```text
HOST_PORT:CONTAINER_PORT
```

Therefore:

```text
8087:80
```

means:

```text
Host → 8087
Container → 80
```

So the Apache server running on port `80` inside the container can be accessed through port `8087` on the host.

The main command to start the application was:

```bash
docker compose up -d
```

The main verification command was:

```bash
docker ps
```

and the Apache test was:

```bash
curl http://localhost:8087
```

**Day 44 completed successfully.** 
