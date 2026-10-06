# Day 42 - Commands

This file contains the commands used during **Day 42 of my 100 Days of DevOps Challenge**.

The objective was to create a Docker network with a specific subnet and IP range.

---

# Step 1: Check Existing Docker Networks

```bash
docker network ls
```

This lists all Docker networks currently available on the server.

---

# Step 2: Create the Docker Network

Use the following command with the exact values provided by the KodeKloud task:

```bash
docker network create \
  --driver bridge \
  --subnet <SUBNET> \
  --ip-range <IP-RANGE> \
  <NETWORK-NAME>
```

Example format:

```bash
docker network create \
  --driver bridge \
  --subnet <required-subnet> \
  --ip-range <required-ip-range> \
  <required-network-name>
```

---

# Step 3: Verify Docker Networks

```bash
docker network ls
```

Check that the newly created network appears in the list.

---

# Step 4: Inspect the Docker Network

```bash
docker network inspect <NETWORK-NAME>
```

This displays detailed information about the network.

---

# Step 5: Verify Subnet and IP Range

```bash
docker network inspect <NETWORK-NAME> | grep -E "Subnet|IPRange"
```

This verifies the configured subnet and IP range.

---

# Quick Command Summary

```bash
# List existing Docker networks
docker network ls

# Create custom Docker network
docker network create \
  --driver bridge \
  --subnet <SUBNET> \
  --ip-range <IP-RANGE> \
  <NETWORK-NAME>

# Verify network
docker network ls

# Inspect network
docker network inspect <NETWORK-NAME>

# Verify subnet and IP range
docker network inspect <NETWORK-NAME> | grep -E "Subnet|IPRange"
```

---

# Important Docker Network Options

## `--driver bridge`

```bash
--driver bridge
```

Creates the network using Docker's bridge network driver.

---

## `--subnet`

```bash
--subnet <SUBNET>
```

Defines the subnet that will be used by the Docker network.

---

## `--ip-range`

```bash
--ip-range <IP-RANGE>
```

Defines the IP address pool from which Docker can allocate container IP addresses.

---

## Network Name

```bash
<NETWORK-NAME>
```

Defines the name of the custom Docker network.

---

# Important Lesson

The most important command from this challenge was:

```bash
docker network create \
  --driver bridge \
  --subnet <SUBNET> \
  --ip-range <IP-RANGE> \
  <NETWORK-NAME>
```

The subnet, IP range, and network name must be replaced with the **exact values provided in the KodeKloud task**.

After creating the network, always verify it using:

```bash
docker network inspect <NETWORK-NAME>
```

This confirms that the Docker network was created with the required configuration.

**Day 42 completed successfully.**
