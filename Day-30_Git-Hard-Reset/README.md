# Day 30 - Reset Git Commit History

## Challenge Overview

This is Day 30 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Git Commit History Management**. The Nautilus application development team had a test Git repository on the **Storage Server** in the Stratos Data Center.

The repository contained several test commits that the development team wanted to clean up. The requirement was to reset the repository so that only the required commits remained in the history.

### Challenge Requirement

The Git repository was located at:

```text
/usr/src/kodekloudrepos/blog
```

The requirement was to reset the Git commit history so that only these **two commits** remained:

1. `initial commit`
2. `add data.txt file`

The repository's `HEAD` and branch also needed to point to the commit with the message:

```text
add data.txt file
```

After resetting the repository, the changes had to be pushed to the remote repository.

---

## Objectives

The objectives of this challenge are:

* Access the Storage Server using SSH.
* Navigate to the Git repository.
* Inspect the existing Git commit history.
* Identify the commit with message `add data.txt file`.
* Reset the branch and work tree to the required commit.
* Remove the unwanted commits from the current branch history.
* Verify that only the required two commits remain.
* Verify the working tree after the reset.
* Push the updated history to the remote repository.
* Understand how `git reset --hard` changes `HEAD`, the index, and the working tree.

---

## Environment

| Item             | Details                        |
| ---------------- | ------------------------------ |
| Challenge        | 100 Days of DevOps             |
| Platform         | KodeKloud                      |
| Day              | 30                             |
| Server           | Storage Server                 |
| Repository       | `/usr/src/kodekloudrepos/blog` |
| Git Operation    | Reset Commit History           |
| Required Commits | 2                              |
| Target Commit    | `add data.txt file`            |
| Push Required    | Yes                            |
| Status           | Completed                      |

---

# Solution

## Step 1: Access the Storage Server

First, I accessed the **Storage Server** using SSH.

The SSH command format is:

```bash
ssh <username>@<storage-server-hostname>
```

Example:

```bash
ssh natasha@ststor01
```

---

## Step 2: Navigate to the Git Repository

After connecting to the Storage Server, I navigated to the required repository:

```bash
cd /usr/src/kodekloudrepos/blog
```

---

## Step 3: Check the Current Git Status

Before making any changes, I checked the current repository status:

```bash
git status
```

This helped me understand the current state of the working tree.

---

## Step 4: Check the Existing Commit History

I inspected the existing commit history:

```bash
git log --oneline
```

The repository contained more commits than the two commits required by the challenge.

I needed to find the commit with the message:

```text
add data.txt file
```

For easier identification, I used:

```bash
git log --oneline --all
```

---

## Step 5: Identify the Required Commit

From the Git history, I located the commit whose message was:

```text
add data.txt file
```

This was the commit that the branch needed to point to after the reset.

The history needed to look like:

```text
add data.txt file
initial commit
```

with no additional commits after `add data.txt file`.

---

## Step 6: Reset the Repository

After identifying the correct commit, I reset the current branch to that commit.

I used:

```bash
git reset --hard <commit-id>
```

The `<commit-id>` represents the commit containing:

```text
add data.txt file
```

The `--hard` option was important because the task required cleaning the **commit history and work tree**.

It resets:

* `HEAD`
* The current branch
* The staging area
* The working tree

to the selected commit.

---

## Step 7: Verify the Commit History

After resetting, I checked the commit history again:

```bash
git log --oneline
```

The expected result was only two commits:

```text
<commit-id> add data.txt file
<commit-id> initial commit
```

This confirmed that the unwanted commits had been removed from the current branch history.

---

## Step 8: Verify the Working Tree

I checked the repository status:

```bash
git status
```

The working tree should be clean after the hard reset.

This confirmed that the repository's working tree matched the selected commit.

---

## Step 9: Verify HEAD

I verified the current `HEAD`:

```bash
git rev-parse HEAD
```

The resulting commit ID should match the commit containing:

```text
add data.txt file
```

I could also verify the latest commit using:

```bash
git log -1 --oneline
```

Expected result:

```text
<commit-id> add data.txt file
```

---

## Step 10: Push the Changes

Because the local branch history had been rewritten, I needed to push the updated history to the remote repository.

I first checked the current branch:

```bash
git branch --show-current
```

Then I pushed the rewritten history:

```bash
git push --force
```

The force push was necessary because `git reset --hard` moved the branch backward and changed the commit history.

---

# Git Reset Explanation

The main command used in this challenge was:

```bash
git reset --hard <commit-id>
```

This moved the current branch and `HEAD` back to the selected commit.

### `HEAD`

`HEAD` represents the current commit that the checked-out branch points to.

### `--hard`

The `--hard` option updates all three areas:

```text
HEAD
Index / Staging Area
Working Tree
```

Therefore, files and changes introduced by commits after the selected commit were also removed from the working tree.

---

# Final Git History

The final repository history should contain only:

```text
add data.txt file
initial commit
```

The target commit:

```text
add data.txt file
```

should be the current `HEAD`.

---

# Verification

The final verification commands were:

```bash
git log --oneline
```

```bash
git status
```

```bash
git log -1 --oneline
```

```bash
git rev-parse HEAD
```

The repository should show only the two required commits, with `add data.txt file` as the latest commit.

---

# What I Learned

From this challenge, I learned and practiced:

* How to inspect Git commit history.
* How to identify a specific commit by its commit message.
* How `git reset` works.
* How `git reset --hard` affects the working tree.
* How to move `HEAD` and a branch back to an earlier commit.
* How to remove unwanted commits from the current branch history.
* How to verify the Git working tree after a reset.
* Why rewriting Git history can require a force push.
* How to verify the current `HEAD`.
* The importance of carefully identifying the correct commit before performing a destructive Git operation.

# Challenge Status

Day 30 — Completed Successfully 

**Git history cleaned, the branch reset to the required commit, and the updated history pushed successfully.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
