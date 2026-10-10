# Day 46 - Commands

This file contains the commands used during **Day 46 of my 100 Days of DevOps Challenge by KodeKloud**.

The objective was to deploy an application using Docker Compose with two services, `web` and `db`, configure the web dependency, bind ports, and add volumes to both services.

---

## Step 1: Verify Docker

```bash
docker --version
```

---

## Step 2: Verify Docker Compose

For standalone Docker Compose:

```bash
docker-compose --version
```

For the newer Compose plugin:

```bash
docker compose version
```

---

## Step 3: Create a Project Directory

```bash
mkdir docker-compose-app
cd docker-compose-app
```

---

## Step 4: Create the Compose File

```bash
nano docker-compose.yml
```

Save the required service configuration in this file.

---

## Step 5: Validate the Compose File

For standalone Docker Compose:

```bash
docker-compose config
```

For the newer Compose plugin:

```bash
docker compose config
```

---

## Step 6: Start the Services

```bash
docker-compose up -d
```

Or:

```bash
docker compose up -d
```

The `-d` option runs the services in the background.

---

## Step 7: Verify Running Services

```bash
docker-compose ps
```

Or:

```bash
docker compose ps
```

---

## Step 8: Check Container Logs

View logs from all services:

```bash
docker-compose logs
```

Follow logs continuously:

```bash
docker-compose logs -f
```

View only the web service logs:

```bash
docker-compose logs web
```

View only the database service logs:

```bash
docker-compose logs db
```

---

## Step 9: Check Running Containers

```bash
docker ps
```

To display all containers, including stopped ones:

```bash
docker ps -a
```

---

## Step 10: Test Port Binding

For a web service mapped to host port `8080`:

```bash
curl http://localhost:8080
```

Replace `8080` with the actual host port from the Compose configuration.

---

## Step 11: Inspect Volumes

List Docker volumes:

```bash
docker volume ls
```

Inspect a named volume:

```bash
docker volume inspect <volume-name>
```

---

## Step 12: Stop the Application

```bash
docker-compose down
```

Or:

```bash
docker compose down
```

This removes the Compose containers and network while normally preserving named volumes.

---

# Quick Command Summary

```bash
# Check Docker
docker --version

# Check Compose
docker-compose --version

# Create project directory
mkdir docker-compose-app
cd docker-compose-app

# Create Compose file
nano docker-compose.yml

# Validate Compose configuration
docker-compose config

# Start services
docker-compose up -d

# Verify services
docker-compose ps

# Check running containers
docker ps

# Check logs
docker-compose logs

# Check web service logs
docker-compose logs web

# Check database service logs
docker-compose logs db

# List volumes
docker volume ls

# Stop and remove containers and network
docker-compose down
```

---

# Important Docker Compose Concepts

## 1. Service Dependency

```yaml
depends_on:
  - db
```

Configures the web service to start after the database service is started by Compose. It does not guarantee database readiness.

## 2. Port Binding

```yaml
ports:
  - "8080:80"
```

Maps host port `8080` to container port `80`.

## 3. Volumes

```yaml
volumes:
  - db_data:/var/lib/mysql
```

Mounts a named volume at the specified database directory.

## 4. Detached Mode

```bash
docker-compose up -d
```

Runs the application in the background.

## 5. Service Verification

```bash
docker-compose ps
```

Displays the status of the Compose services.

---

# Important Lesson

Docker Compose simplifies application deployment by allowing multiple services to be defined and managed together.

For this challenge, the key requirements were:

- Two services: `web` and `db`.
- A dependency from `web` to `db`.
- Port binding for both services as required.
- Volumes for both services.
- Successful deployment and verification.

**Day 46 completed successfully.**
