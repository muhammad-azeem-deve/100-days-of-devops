# Day 28 - Merge a Specific Commit from Feature Branch

## Challenge Overview

This is Day 28 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Git Branching, Commit Management, and Cherry-Picking**.

The Nautilus application development team was working on a Git repository located at:

```text
/opt/demo.git
```

The repository was cloned on the **Storage Server** at:

```text
/usr/src/kodekloudrepos
```

There were two branches in the repository:

```text
master
feature
```

A developer was working on the `feature` branch, but their work was still in progress. However, they wanted to merge **only one specific commit** from the `feature` branch into the `master` branch.

The commit that needed to be merged had the message:

```text
Update info.txt
```

The task also required pushing the changes after successfully merging the commit.

---

## Challenge Requirement

Merge only the commit with the message:

```text
Update info.txt
```

from the `feature` branch into the `master` branch.

The complete task required:

* Access the Storage Server.
* Navigate to the Git repository.
* Check the available branches.
* Identify the commit with the message `Update info.txt`.
* Switch to the `master` branch.
* Cherry-pick the required commit.
* Verify the commit and changes.
* Push the updated `master` branch to the remote repository.

---

## Objectives

The objectives of this challenge are:

* Access the Storage Server using SSH.
* Navigate to the repository directory.
* Understand the repository structure.
* Check available Git branches.
* Inspect the commit history.
* Find a specific commit using its commit message.
* Understand the purpose of `git cherry-pick`.
* Apply only one commit from another branch.
* Verify the changes after cherry-picking.
* Push the changes to the remote repository.
* Avoid merging the entire `feature` branch into `master`.

---

## Environment

| Item            | Details                   |
| --------------- | ------------------------- |
| Challenge       | 100 Days of DevOps        |
| Platform        | KodeKloud                 |
| Day             | 28                        |
| Server          | Storage Server            |
| Repository      | `/opt/demo.git`           |
| Clone Location  | `/usr/src/kodekloudrepos` |
| Main Branch     | `master`                  |
| Feature Branch  | `feature`                 |
| Required Commit | `Update info.txt`         |
| Git Operation   | Cherry-pick               |
| Status          | Completed                 |

---

# Solution

## Step 1: Access the Storage Server

First, I accessed the **Storage Server** using SSH.

The SSH command format was:

```bash
ssh <username>@<storage-server>
```

After connecting, I verified the server:

```bash
hostname
```

---

## Step 2: Navigate to the Repository

The repository was cloned at:

```text
/usr/src/kodekloudrepos
```

I navigated to the repository:

```bash
cd /usr/src/kodekloudrepos
```

---

## Step 3: Check Git Status

Before making any changes, I checked the current Git status:

```bash
git status
```

This helped verify the current branch and whether there were any uncommitted changes.

---

## Step 4: Check Available Branches

I checked the available branches:

```bash
git branch
```

The repository contained:

```text
master
feature
```

I could also check both local and remote branches using:

```bash
git branch -a
```

---

## Step 5: Find the Required Commit

The task specifically required the commit with the message:

```text
Update info.txt
```

I checked the commit history:

```bash
git log --oneline --all
```

This displayed the commit history from the available branches.

I located the commit whose message was:

```text
Update info.txt
```

and noted its commit ID.

For example:

```text
abc1234 Update info.txt
```

The actual commit ID should be taken from the repository rather than using the example above.

---

## Step 6: Switch to the Master Branch

After identifying the required commit, I switched to the `master` branch:

```bash
git checkout master
```

I verified the current branch:

```bash
git branch
```

The output should show:

```text
* master
  feature
```

---

## Step 7: Cherry-Pick the Required Commit

Instead of merging the entire `feature` branch, I used `git cherry-pick` to apply only the required commit to `master`.

The command was:

```bash
git cherry-pick <commit-id>
```

For example:

```bash
git cherry-pick abc1234
```

The `<commit-id>` must be replaced with the actual commit ID for:

```text
Update info.txt
```

---

## Step 8: Verify the Cherry-Pick

After cherry-picking the commit, I checked the Git history:

```bash
git log --oneline -5
```

The `Update info.txt` commit should now appear in the `master` branch history.

I also checked the repository status:

```bash
git status
```

---

## Step 9: Verify the File Changes

Since the required commit updated `info.txt`, I verified the file:

```bash
cat info.txt
```

I could also check the changes introduced by the commit:

```bash
git show <commit-id>
```

---

## Step 10: Push the Changes

After successfully cherry-picking and verifying the commit, I pushed the updated `master` branch:

```bash
git push origin master
```

This pushed the updated `master` branch to the remote repository.

---

# Why Cherry-Pick Was Used

The developer did **not** want the entire `feature` branch merged into `master`.

The requirement was to move only one specific commit:

```text
Update info.txt
```

Therefore, `git cherry-pick` was the appropriate Git operation.

Conceptually:

```text
feature branch

A ── B ── C ── D
          ↑
    Update info.txt


master branch

A ── B

          │
          │ cherry-pick C
          ↓

A ── B ── C'
```

The commit is applied to `master` without merging the remaining work from the `feature` branch.

---

# Important Git Commands

### Check Branches

```bash
git branch
```

### View All Branches

```bash
git branch -a
```

### View Commit History

```bash
git log --oneline --all
```

### Switch to Master

```bash
git checkout master
```

### Cherry-Pick a Specific Commit

```bash
git cherry-pick <commit-id>
```

### Check Status

```bash
git status
```

### View Commit Details

```bash
git show <commit-id>
```

### Push Changes

```bash
git push origin master
```

---

# What I Learned

From this challenge, I learned and practiced:

* How Git branches are used for parallel development.
* How to inspect commit history across branches.
* How to find a specific commit using `git log`.
* What `git cherry-pick` does.
* How to apply a single commit from another branch.
* The difference between cherry-picking a commit and merging an entire branch.
* How to verify changes after a cherry-pick.
* How to push the updated branch to a remote repository.
* Why it is important to carefully identify the correct commit before applying it.

# Challenge Status

Day 28 — Completed Successfully 

**Specific commit merged from the `feature` branch into `master` using Git cherry-pick, and the changes were pushed successfully.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
