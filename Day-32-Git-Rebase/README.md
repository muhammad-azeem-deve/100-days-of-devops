# Day 32 - Rebase Feature Branch with Master

## Challenge Overview

This is Day 32 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Git Branching and Rebasing**. The Nautilus application development team was working on a project repository named `games.git`.

The repository was cloned on the **Storage Server** at:

```text
/usr/src/kodekloudrepos
```

The requirement was to rebase the developer's feature branch with the latest changes from the `master` branch without losing any feature-branch changes.

The team specifically did not want to merge `master` into the feature branch because that would create a merge commit.

---

## Challenge Requirement

The developer was working on a feature branch and their work was still in progress.

Meanwhile, some new changes had been pushed to the `master` branch.

The requirement was to:

* Rebase the feature branch with the latest `master` branch.
* Preserve all existing changes from the feature branch.
* Do not create a merge commit.
* Complete the task using Git rebase.
* Push the updated feature branch after completing the rebase.

---

## Objectives

The objectives of this challenge are:

* Access the Storage Server.
* Navigate to the Git repository.
* Identify the available branches.
* Update the local `master` branch.
* Switch to the developer's feature branch.
* Rebase the feature branch onto `master`.
* Resolve conflicts if required.
* Verify the Git history.
* Push the rebased feature branch to the remote repository.
* Understand the difference between `merge` and `rebase`.

---

## Environment

| Item                | Details                   |
| ------------------- | ------------------------- |
| Challenge           | 100 Days of DevOps        |
| Platform            | KodeKloud                 |
| Day                 | 32                        |
| Repository          | `games.git`               |
| Repository Location | `/usr/src/kodekloudrepos` |
| Main Branch         | `master`                  |
| Working Branch      | Feature Branch            |
| Operation           | Git Rebase                |
| Merge Commit        | Not Required              |
| Push Changes        | Required                  |
| Status              | Completed                 |

---

# Solution

## Step 1: Access the Storage Server

First, I accessed the **Storage Server** using SSH.

The SSH command format was:

```bash
ssh <username>@<storage-server-hostname>
```

After logging in, I verified the server:

```bash
hostname
```

---

## Step 2: Navigate to the Repository

The repository was located under:

```text
/usr/src/kodekloudrepos
```

I navigated to the repository:

```bash
cd /usr/src/kodekloudrepos/games
```

---

## Step 3: Check the Git Status

Before making any changes, I checked the repository status:

```bash
git status
```

This helped confirm the current branch and whether there were any uncommitted changes.

---

## Step 4: Check Available Branches

I checked the available branches:

```bash
git branch
```

This allowed me to identify the developer's feature branch and the `master` branch.



---


## Step 6: Switch to the Feature Branch

After updating `master`, I switched back to the developer's feature branch:

```bash
git checkout <feature-branch>
```

I verified the current branch:

```bash
git branch
```

The `*` symbol indicates the currently active branch.

---

## Step 7: Rebase the Feature Branch

I rebased the feature branch onto the latest `master`:

```bash
git rebase master
```

Git temporarily removed the feature branch commits, moved the branch on top of the latest `master`, and then reapplied the feature commits.

This preserved the feature work while avoiding a merge commit.

---

## Step 8: Handle Conflicts if Required

If Git reports a conflict during the rebase, I would first check the conflicting files:

```bash
git status
```

After resolving the conflicts manually, I would stage the resolved files:

```bash
git add <file>
```

Then continue the rebase:

```bash
git rebase --continue
```

If there were multiple conflicts, the process would be repeated until the rebase completed.

If the rebase needed to be cancelled:

```bash
git rebase --abort
```

This returns the branch to its state before the rebase started.

---

## Step 9: Verify the Rebase

After the rebase completed, I checked the commit history:

```bash
git log --oneline --graph --all
```

I also checked the repository status:

```bash
git status
```

The feature branch should now contain the latest `master` changes followed by the feature commits.

Most importantly, there should be no additional merge commit created by this operation.

---

## Step 10: Push the Rebasing Changes

Because rebase rewrites the feature branch's commit history, I pushed the updated branch using:

```bash
git push --force-with-lease origin <feature-branch>
```

`--force-with-lease` is safer than using `--force` because it checks that the remote branch has not changed unexpectedly.

---

# Understanding Rebase

Suppose the history initially looks like:

```text
A---B---C        master
     \
      D---E      feature
```

Here:

* `A` and `B` are common commits.
* `C` is a new commit on `master`.
* `D` and `E` are feature commits.

After running:

```bash
git rebase master
```

the history becomes conceptually:

```text
A---B---C---D'---E'      feature
         ^
       master
```

The feature commits are reapplied on top of the latest `master`.

The commits may receive new IDs because rebase creates new commit objects.

---

# Why Rebase Was Required

The task specifically stated that the developer did not want to merge `master` into the feature branch because they did not want an additional merge commit.

Using:

```bash
git merge master
```

could create a merge commit.

Instead, I used:

```bash
git rebase master
```

This keeps the commit history linear and applies the feature work on top of the latest master history.

---

# What I Learned

From this challenge, I learned and practiced:

* How Git branches work.
* How to identify local and remote branches.
* How to update a local `master` branch.
* How Git rebase works.
* The difference between `merge` and `rebase`.
* How rebase preserves feature changes while changing their position in history.
* How to resolve conflicts during a rebase.
* How to continue a rebase using `git rebase --continue`.
* How to cancel a rebase using `git rebase --abort`.
* How to inspect Git history using `git log --graph`.
* Why rebasing can require a force push.
* Why `--force-with-lease` is safer than `--force`.

# Challenge Status

Day 32 — Completed Successfully 

**Feature branch successfully rebased with the latest master branch without creating a merge commit.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
