# Day 22 - Clone Git Repository

## Challenge Overview

This is Day 22 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Git Repository Management** and required cloning an existing Git repository from the local path on the **Storage Server**.

The repository was currently unused, but the Nautilus application development team required a copy of it in the specified directory.

### Challenge Requirement

The task was to clone the following Git repository:

```text
/opt/media.git
```

into:

```text
/usr/src/kodekloudrepos
```

The task had to be performed using the **`natasha` user**.

No modifications were allowed to the repository or existing directories, including changing permissions or making unauthorized alterations.

---

## Objectives

The objectives of this challenge are:

* Access the Storage Server using SSH.
* Work with the `natasha` user.
* Navigate to the required destination directory.
* Clone the existing Git repository.
* Clone the repository without modifying its contents.
* Ensure that existing directories and permissions are not changed.
* Verify that the repository was cloned successfully.
* Understand how to clone a local Git repository.
* Understand the importance of following existing permission and configuration requirements.

---

## Environment

| Item              | Details                   |
| ----------------- | ------------------------- |
| Challenge         | 100 Days of DevOps        |
| Platform          | KodeKloud                 |
| Day               | 22                        |
| Server            | Storage Server            |
| User              | `natasha`                 |
| Source Repository | `/opt/media.git`          |
| Destination       | `/usr/src/kodekloudrepos` |
| Operation         | Git Clone                 |
| Repository Type   | Local Git Repository      |
| Status            | Completed                 |

---

# Solution

## Step 1: Access the Storage Server

First, I accessed the **Storage Server** using SSH.

The SSH command format is:

```bash
ssh <username>@<storage-server-hostname>
```

I logged in using the `natasha` user as required by the challenge.

---

## Step 2: Verify the Current User

After connecting to the Storage Server, I verified that I was working as the required user:

```bash
whoami
```

Expected output:

```text
natasha
```

This was important because the challenge specifically required the repository to be cloned using the `natasha` user.

---

## Step 3: Navigate to the Destination Directory

The repository needed to be cloned into:

```text
/usr/src/kodekloudrepos
```

I navigated to the destination directory:

```bash
cd /usr/src/kodekloudrepos
```

Then I verified the current location:

```bash
pwd
```

Expected output:

```text
/usr/src/kodekloudrepos
```

---

## Step 4: Check the Destination Directory

Before cloning the repository, I checked the existing contents of the directory:

```bash
ls -la
```

This helped ensure that I did not make unnecessary modifications to existing files or directories.

---

## Step 5: Clone the Git Repository

The source repository was located at:

```text
/opt/media.git
```

From inside the destination directory, I cloned the repository using:

```bash
git clone /opt/media.git
```

Git created a working copy of the repository inside:

```text
/usr/src/kodekloudrepos/media
```

---

## Step 6: Verify the Cloned Repository

After cloning, I checked the destination directory:

```bash
ls -la
```

I verified that the `media` repository directory was present.

I then entered the cloned repository:

```bash
cd media
```

and checked its contents:

```bash
ls -la
```

---

## Step 7: Verify Git Repository Status

I verified that the cloned directory was a valid Git repository:

```bash
git status
```

The command confirmed that the repository had been cloned successfully and that there were no unnecessary modifications.

I also checked the configured remote repository:

```bash
git remote -v
```

This confirmed that the repository was cloned from:

```text
/opt/media.git
```

---

# Important Requirement

The challenge specifically stated that **no modifications should be made to the repository or existing directories**.

Therefore, I did not:

* Change directory permissions.
* Change repository permissions.
* Modify existing files.
* Delete existing files.
* Rename existing directories.
* Make unauthorized configuration changes.

I only performed the required Git clone operation.

---

# Git Clone Process

The overall process was:

```text
Access Storage Server
        ↓
Login as natasha
        ↓
Navigate to /usr/src/kodekloudrepos
        ↓
Clone /opt/media.git
        ↓
Verify cloned repository
        ↓
Check Git status
        ↓
Task Completed
```

---

# What I Learned

From this challenge, I learned and practiced:

* How to access a remote Linux server using SSH.
* How to verify the current Linux user using `whoami`.
* How to navigate Linux directories using `cd`.
* How to verify the current directory using `pwd`.
* How to clone a Git repository using `git clone`.
* How to clone a repository from a local filesystem path.
* How to verify a cloned Git repository using `git status`.
* How to check the configured Git remote using `git remote -v`.
* The importance of following user-specific requirements.
* The importance of not changing permissions or making unnecessary modifications during infrastructure tasks.

# Challenge Status

Day 22 — Completed Successfully 

**Repository cloned successfully on the Storage Server using the `natasha` user without making unauthorized modifications.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
