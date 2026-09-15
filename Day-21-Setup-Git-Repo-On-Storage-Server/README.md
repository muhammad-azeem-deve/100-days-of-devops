# Day 21 - Install Git and Create a Bare Repository

## Challenge Overview

This is Day 21 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Git Installation and Bare Repository Management**. The task required installing Git on the **Storage Server** and creating a bare Git repository at the specified location.

To complete the challenge, I first accessed the Storage Server using SSH, installed Git using the `yum` package manager, created a bare repository named `news.git`, and finally verified that the repository was actually configured as a bare repository.

### Challenge Requirement

Install **Git** on the **Storage Server** and create a **bare Git repository** with the following path:

```text
/opt/news.git
```

The repository must be a **bare repository**, meaning it should not contain a normal working tree.

---

## Objectives

The objectives of this challenge are:

* Access the Storage Server using SSH.
* Verify that the correct server has been accessed.
* Install Git using the `yum` package manager.
* Verify that Git was installed successfully.
* Create a bare Git repository.
* Store the repository at `/opt/news.git`.
* Verify that the repository is a bare repository.
* Understand the difference between a normal Git repository and a bare Git repository.
* Learn how bare repositories can be used as central remote repositories.

---

## Environment

| Item            | Details            |
| --------------- | ------------------ |
| Challenge       | 100 Days of DevOps |
| Platform        | KodeKloud          |
| Day             | 21                 |
| Server          | Storage Server     |
| Software        | Git                |
| Package Manager | `yum`              |
| Repository Type | Bare Repository    |
| Repository Name | `news.git`         |
| Repository Path | `/opt/news.git`    |
| Status          | Completed          |

---

# Solution

## Step 1: Access the Storage Server

First, I accessed the **Storage Server** using SSH.

The SSH command format is:

```bash
ssh <username>@<storage-server-hostname>
```

After connecting, I verified that I was working on the correct server.

I used:

```bash
hostname
```

---

## Step 2: Install Git

After accessing the Storage Server, I installed Git using the `yum` package manager.

I used:

```bash
sudo yum install git -y
```

The `-y` option automatically confirms the installation prompts.

---

## Step 3: Verify Git Installation

After the installation was completed, I verified that Git was installed successfully:

```bash
git --version
```

The command displayed the installed Git version.

---

## Step 4: Create the Bare Repository

Next, I created the required bare repository at:

```text
/opt/news.git
```

I used:

```bash
sudo git init --bare /opt/news.git
```

The `--bare` option creates a Git repository without a working directory.

---

## Step 5: Verify the Bare Repository

After creating the repository, I checked its contents:

```bash
ls -la /opt/news.git
```

A bare repository contains Git repository files and directories such as:

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

Unlike a normal Git repository, there is no separate working tree containing project files.

---

## Step 6: Verify Using Git Command

I also verified that `/opt/news.git` was a bare repository using:

```bash
git -C /opt/news.git rev-parse --is-bare-repository
```

Expected output:

```text
true
```

This confirmed that the repository was successfully created as a **bare Git repository**.

---

# Bare Git Repository

A **bare Git repository** contains the Git database but does not contain a working directory.

A normal repository usually contains:

```text
project/
├── .git/
├── file1
├── file2
└── README.md
```

A bare repository contains the Git repository data directly:

```text
news.git/
├── HEAD
├── branches
├── config
├── description
├── hooks
├── info
├── objects
└── refs
```

Bare repositories are commonly used as **central remote repositories** where developers push and pull code.

---

# Why Use a Bare Repository?

A bare repository is useful when a server is being used as a central Git repository.

For example:

```text
Developer 1
     |
     | git push
     v
/opt/news.git
     ^
     | git push
     |
Developer 2
```

The server stores the Git history and branches, while developers maintain their own working copies.

---

# Verification

The important verification command was:

```bash
git -C /opt/news.git rev-parse --is-bare-repository
```

Expected output:

```text
true
```

This confirmed that `/opt/news.git` was a bare repository.

Git installation was also verified using:

```bash
git --version
```

---

# What I Learned

From this challenge, I learned and practiced:

* How to access a remote Linux server using SSH.
* How to install Git using `yum`.
* How to verify Git installation.
* How to create a bare Git repository.
* How to use `git init --bare`.
* How to verify whether a repository is bare.
* The difference between a normal Git repository and a bare repository.
* Why bare repositories are useful as central remote repositories.
* How Git repositories can be hosted on a Linux server.
* How DevOps environments use Git repositories for source-code management.

# Challenge Status

Day 21 — Completed Successfully 

**Git installed successfully and the `/opt/news.git` bare repository was created and verified.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
