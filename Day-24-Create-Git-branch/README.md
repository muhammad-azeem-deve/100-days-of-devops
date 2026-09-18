# Day 24 - Create a New Git Branch

## Challenge Overview

This is Day 24 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Git Branching**. The Nautilus developers were working on a project repository and wanted to maintain some upcoming changes in a separate branch without modifying the existing code.

### Challenge Requirement

On the **Storage Server** in the **Stratos Data Center**, create a new branch named:

```text
xfusioncorp_official
```

The branch must be created from the existing `master` branch in the following Git repository:

```text
/usr/src/kodekloudrepos/official
```

No changes should be made to the application code.

---

## Objectives

The objectives of this challenge are:

* Access the Storage Server using SSH.
* Navigate to the required Git repository.
* Verify the current Git branch.
* Make sure the branch is based on `master`.
* Create a new branch named `xfusioncorp_official`.
* Verify that the new branch was created successfully.
* Ensure that no changes are made to the project code.
* Understand how Git branches are created from an existing branch.

---

## Environment

| Item         | Details                            |
| ------------ | ---------------------------------- |
| Challenge    | 100 Days of DevOps                 |
| Platform     | KodeKloud                          |
| Day          | 24                                 |
| Server       | Storage Server                     |
| Data Center  | Stratos DC                         |
| Repository   | `/usr/src/kodekloudrepos/official` |
| Base Branch  | `master`                           |
| New Branch   | `xfusioncorp_official`             |
| Code Changes | None                               |
| Status       | Completed                          |

---

# Solution

## Step 1: Access the Storage Server

First, I accessed the **Storage Server** using SSH.

The SSH command format is:

```bash
ssh <username>@<storage-server-hostname>
```

After connecting, I verified that I was on the correct server using:

```bash
hostname
```

---

## Step 2: Navigate to the Git Repository

The required repository was located at:

```text
/usr/src/kodekloudrepos/official
```

I navigated to the repository using:

```bash
cd /usr/src/kodekloudrepos/official
```

---

## Step 3: Check the Git Repository Status

Before creating the branch, I checked the current repository status:

```bash
git status
```

This allowed me to verify the current branch and make sure I was not intentionally making any code changes.

---

## Step 4: Check the Existing Branch

I checked the available branches using:

```bash
git branch
```

The repository had the `master` branch, which was the required base branch.

---

## Step 5: Switch to the Master Branch

To make sure the new branch was created from `master`, I switched to the master branch:

```bash
git checkout master
```

I verified the active branch again:

```bash
git branch
```

The `master` branch should be marked with `*`.

---

## Step 6: Create the New Branch

I created the required branch from `master`:

```bash
git checkout -b xfusioncorp_official
```

This command creates the new branch and switches to it immediately.

The new branch was:

```text
xfusioncorp_official
```

---

## Step 7: Verify the New Branch

I verified the branches using:

```bash
git branch
```

The output should show both branches:

```text
* xfusioncorp_official
  master
```

The `*` indicates that `xfusioncorp_official` is the currently active branch.

---

## Step 8: Verify Repository Status

Finally, I checked the repository status:

```bash
git status
```

This confirmed that I was working on the new branch and that no code changes had been made.

The important point was that the task only required **creating the branch**, so I did not modify any project files.

---

# Git Branching Concept

A Git branch allows developers to work on a separate line of development without directly modifying another branch.

In this challenge:

```text
master
   |
   |
   +------ xfusioncorp_official
```

The new branch was created from the current state of `master`.

This allows developers to work on new features separately while keeping the existing `master` branch unchanged.

---

# What I Learned

From this challenge, I learned and practiced:

* How to access a remote Linux server using SSH.
* How to navigate to a Git repository.
* How to check Git repository status.
* How to list Git branches using `git branch`.
* How to switch between branches.
* How to create a new branch using `git checkout -b`.
* How to create a branch from the `master` branch.
* How to verify the currently active Git branch.
* The importance of checking the repository before making changes.
* How Git branches allow developers to work on new features separately.
* Why creating a separate branch is useful for maintaining clean development workflows.

# Challenge Status

Day 24 — Completed Successfully 

**Created `xfusioncorp_official` from `master` without making any changes to the project code.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
