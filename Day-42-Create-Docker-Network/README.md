# Day 42 - Create a Docker Network with Specific Subnets and IP Range

## Challenge Overview

This is Day 42 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Docker Networking** and required creating a Docker network with a specific subnet and IP range.

The objective was to create a custom Docker network using the required network name, subnet, and IP range, and then verify that the network was created with the correct configuration.

### Challenge Requirement

Create a Docker network with the specific:

- Network name
- Network driver
- Subnet
- IP range

provided in the KodeKloud task.

The Docker network needed to be created successfully and verified using Docker network commands.

---

## Objectives

The objectives of this challenge are:

- Understand Docker networking.
- Create a custom Docker network.
- Use the Docker `bridge` network driver.
- Configure a specific subnet.
- Configure a specific IP range.
- Verify the created Docker network.
- Inspect Docker network configuration.
- Understand the difference between a subnet and an IP range.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 42 |
| Technology | Docker |
| Topic | Docker Networking |
| Network Type | Bridge |
| Subnet | As specified in the task |
| IP Range | As specified in the task |
| Status | Completed |

---

# Solution

## Step 1: Check Existing Docker Networks

Before creating the network, I checked the existing Docker networks:

```bash
docker network ls
```

This helped me understand which Docker networks were already available on the server.

---

## Step 2: Create the Docker Network

I created the custom Docker network using the required subnet and IP range.

The general command was:

```bash
docker network create \
  --driver bridge \
  --subnet <SUBNET> \
  --ip-range <IP-RANGE> \
  <NETWORK-NAME>
```

The values for `<SUBNET>`, `<IP-RANGE>`, and `<NETWORK-NAME>` were replaced with the exact values provided by the KodeKloud task.

For example:

```bash
docker network create \
  --driver bridge \
  --subnet <required-subnet> \
  --ip-range <required-ip-range> \
  <required-network-name>
```

---

## Step 3: Verify the Docker Network

After creating the network, I checked the available Docker networks:

```bash
docker network ls
```

The newly created network appeared in the list.

---

## Step 4: Inspect the Network Configuration

I inspected the newly created network:

```bash
docker network inspect <NETWORK-NAME>
```

This displayed detailed information about the network, including:

- Network name
- Network driver
- Subnet
- IP range
- Gateway
- Connected containers

---

## Step 5: Verify Subnet and IP Range

I specifically checked the subnet and IP range using:

```bash
docker network inspect <NETWORK-NAME> | grep -E "Subnet|IPRange"
```

This allowed me to confirm that the network was created using the required IP configuration.

---

# Understanding Docker Networking

A Docker network allows containers to communicate with each other.

For example:

```text
                 Docker Network
                       |
          +------------+------------+
          |            |            |
      Container 1  Container 2  Container 3
```

A custom network gives better control over how containers communicate.

---

## What is a Subnet?

A subnet defines the overall network address range.

For example:

```text
192.168.10.0/24
```

The subnet represents the larger network available to Docker.

A simple way to remember it:

```text
Subnet = Complete Network
```

---

## What is an IP Range?

The IP range defines the portion of the subnet from which Docker can allocate IP addresses.

For example:

```text
Subnet:
192.168.10.0/24

IP Range:
192.168.10.0/26
```

A simple way to remember it:

```text
Subnet = Larger Network
IP Range = Address Pool Used by Docker
```

---

# Docker Bridge Network

The `bridge` driver was used to create the network:

```bash
--driver bridge
```

The bridge driver is commonly used for communication between containers on the same Docker host.

---

# What I Learned

From this challenge, I learned and practiced:

- How Docker networking works.
- How to create a custom Docker network.
- How to use `docker network create`.
- How to specify a Docker network driver.
- How to configure a custom subnet.
- How to configure a custom IP range.
- How to list Docker networks using `docker network ls`.
- How to inspect a Docker network using `docker network inspect`.
- The difference between a subnet and an IP range.
- How custom Docker networks provide control over container networking.

# Challenge Status

Day 42 — Completed Successfully 

**Docker network created with the required subnet and IP range.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
