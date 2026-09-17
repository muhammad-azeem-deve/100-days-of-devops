# Day 23 - Fork a Repository in Gitea

## Challenge Overview

This is Day 23 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Git Repository Management using Gitea**. The task required accessing the Gitea web interface, logging in with the provided `jon` user, locating a specific repository owned by another user, and creating a fork of that repository under the `jon` account.

### Challenge Requirement

The task was to:

1. Access the **Gitea UI**.
2. Log in using the `jon` user.
3. Explore the available repositories.
4. Find the repository:

```text
sarah/story-bolg
```

5. Fork the repository as the `jon` user.
6. Verify that the repository was successfully forked under the `jon` account.

---

## Objectives

The objectives of this challenge are:

* Access the Gitea web interface.
* Authenticate using the `jon` account.
* Explore repositories available in Gitea.
* Locate a repository owned by another user.
* Understand the purpose of repository forking.
* Fork `sarah/story-bolg` into the `jon` account.
* Verify the forked repository.
* Understand how forks are used for independent development.

---

## Environment

| Item            | Details            |
| --------------- | ------------------ |
| Challenge       | 100 Days of DevOps |
| Platform        | KodeKloud          |
| Day             | 23                 |
| Tool            | Gitea              |
| Interface       | Gitea Web UI       |
| User            | `jon`              |
| Original Owner  | `sarah`            |
| Repository      | `story-bolg`       |
| Full Repository | `sarah/story-bolg` |
| Operation       | Fork               |
| Fork Owner      | `jon`              |
| Status          | Completed          |

---

# Solution

## Step 1: Open the Gitea UI

First, I opened the **Gitea web interface** provided by the KodeKloud environment.

The Gitea UI provides a graphical interface for managing Git repositories, users, branches, commits, issues, and other repository-related activities.

---

## Step 2: Login as jon

I logged into the Gitea interface using the provided `jon` account.

```text
Username: jon
Password: <provided-password>
```

After successful authentication, I accessed the Gitea dashboard.

---

## Step 3: Explore the Repositories

After logging in, I explored the available repositories in the Gitea UI.

I searched for the repository owned by `sarah`:

```text
sarah/story-bolg
```

The repository name was:

```text
story-bolg
```

and the repository owner was:

```text
sarah
```

---

## Step 4: Open the Repository

I opened the `story-bolg` repository owned by `sarah`.

The repository was identified as:

```text
sarah/story-bolg
```

I reviewed the repository information before performing the fork operation.

---

## Step 5: Fork the Repository

From the repository page, I selected the **Fork** option.

Gitea provided the option to create the fork under my current account.

I selected the `jon` account as the fork owner and completed the fork operation.

The repository was then copied from:

```text
sarah/story-bolg
```

to the `jon` account.

The resulting repository was:

```text
jon/story-bolg
```

---

## Step 6: Verify the Fork

After completing the fork operation, I checked the repositories available under the `jon` account.

I verified that the repository now appeared as:

```text
jon/story-bolg
```

This confirmed that the repository had been successfully forked.

---

# Understanding Repository Forking

A **fork** is a copy of an existing repository under another user's account or namespace.

In this challenge:

```text
Original Repository
        |
        v
sarah/story-bolg
        |
        | Fork
        v
jon/story-bolg
```

The original repository remained under:

```text
sarah/story-bolg
```

while the fork was created under:

```text
jon/story-bolg
```

A fork allows a developer to work on their own copy of a project without directly modifying the original repository.

---

# What I Learned

From this challenge, I learned and practiced:

* How to access the Gitea web interface.
* How to log in to Gitea using a user account.
* How to explore repositories in Gitea.
* How to identify a repository by its owner and repository name.
* How to fork a repository using the Gitea UI.
* How a fork differs from the original repository.
* How repositories can be copied into another user's namespace.
* How to verify a successfully created fork.
* The role of repository forking in collaborative Git workflows.

# Challenge Status

Day 23 — Completed Successfully 

**Forked `sarah/story-bolg` successfully into the `jon` account using the Gitea UI.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
