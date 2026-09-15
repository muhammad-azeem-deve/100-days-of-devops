# Day 21 - Commands

This file contains the commands used during **Day 21 of my 100 Days of DevOps Challenge**.

The objective was to install Git on the **Storage Server** and create a bare Git repository at:

```text
/opt/news.git
```

---

# Step 1: Access the Storage Server

Connect to the Storage Server using SSH:

```bash
ssh <username>@<storage-server-hostname>
```

Example:

```bash
ssh natasha@ststor01
```

---

# Step 2: Verify the Server

Check the hostname:

```bash
hostname
```

This confirms that I am working on the Storage Server.

---

# Step 3: Install Git

Install Git using `yum`:

```bash
sudo yum install git -y
```

---

# Step 4: Verify Git Installation

Check the installed Git version:

```bash
git --version
```

Example output:

```text
git version 2.x.x
```

---

# Step 5: Create the Bare Repository

Create the required bare repository:

```bash
sudo git init --bare /opt/news.git
```

The repository is created at:

```text
/opt/news.git
```

---

# Step 6: Verify Repository Directory

Check the contents of the repository:

```bash
ls -la /opt/news.git
```

You should see Git repository files and directories such as:

```text
HEAD
branches
config
description
hooks
info
objects
refs
```

---

# Step 7: Verify That the Repository Is Bare

Use the following command:

```bash
git -C /opt/news.git rev-parse --is-bare-repository
```

Expected output:

```text
true
```

The `true` output confirms that `/opt/news.git` is a bare Git repository.

---

# Quick Command Summary

```bash
# Access Storage Server
ssh <username>@<storage-server-hostname>

# Verify server
hostname

# Install Git
sudo yum install git -y

# Verify Git
git --version

# Create bare repository
sudo git init --bare /opt/news.git

# Check repository contents
ls -la /opt/news.git

# Verify bare repository
git -C /opt/news.git rev-parse --is-bare-repository
```

---


# Important Lesson

A **bare Git repository** is commonly used as a central repository on a server because it stores Git history and references without maintaining a checked-out working tree.

For this challenge, the key command was:

```bash
sudo git init --bare /opt/news.git
```

The repository was then verified using:

```bash
git -C /opt/news.git rev-parse --is-bare-repository
```

Expected output:

```text
true
```

**Day 21 completed successfully.** 
