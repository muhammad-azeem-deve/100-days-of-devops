# Day 27 - Revert Git Repository HEAD to Previous Commit

## Challenge Overview

This is Day 27 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Git Version Control and Commit Reverting**. The Nautilus application development team reported an issue with recent commits in a Git repository located on the **Storage Server** in the Stratos DC.

The task was to revert the latest commit (`HEAD`) and return the repository to the state of the previous commit while keeping the Git history intact.

### Challenge Requirement

The Git repository was located at:

```text
/usr/src/kodekloudrepos/official
```

The requirement was to:

* Access the Storage Server.
* Navigate to the `official` Git repository.
* Identify the latest commit (`HEAD`).
* Identify the previous commit using its initial commit message.
* Revert the latest commit.
* Create a new revert commit.
* Use the commit message:

```text
revert official
```

The commit message had to be written entirely in **lowercase letters**.

---

## Objectives

The objectives of this challenge are:

* Access the Storage Server using SSH.
* Navigate to an existing Git repository.
* Check the current Git status.
* Inspect the repository commit history.
* Identify the latest and previous commits.
* Revert the latest commit without deleting Git history.
* Create a new commit with the required message.
* Verify the final Git history.
* Understand the difference between `git revert` and `git reset`.

---

## Environment

| Item                | Details                            |
| ------------------- | ---------------------------------- |
| Challenge           | 100 Days of DevOps                 |
| Platform            | KodeKloud                          |
| Day                 | 27                                 |
| Server              | Storage Server                     |
| Repository          | `/usr/src/kodekloudrepos/official` |
| Operation           | Revert latest commit               |
| Target              | Previous commit                    |
| Revert Message      | `revert official`                  |
| Commit Message Case | Lowercase                          |
| Status              | Completed                          |

---

# Solution

## Step 1: Access the Storage Server

First, I accessed the **Storage Server** using SSH.

The SSH command format is:

```bash
ssh <username>@<storage-server-hostname>
```

After connecting to the server, I verified that I was working on the correct server.

```bash
hostname
```

---

## Step 2: Navigate to the Git Repository

The repository was located at:

```text
/usr/src/kodekloudrepos/official
```

I navigated to the repository using:

```bash
cd /usr/src/kodekloudrepos/official
```

---

## Step 3: Check the Git Status

Before making any changes, I checked the repository status:

```bash
git status
```

This helped confirm the current state of the working tree before performing the revert.

---

## Step 4: Check the Commit History

I inspected the recent commits using:

```bash
git log -n 4
```

This displayed the latest commits in a compact format.

The output allowed me to identify:

* The current `HEAD` commit.
* The previous commit.
* The commit message associated with the previous commit.

The task specifically required reverting the latest commit and returning the repository content to the previous commit's state.

---

## Step 5: Revert the Latest Commit

After identifying the latest commit, I reverted it using:

```bash
git revert HEAD
```

Git created a new commit that reverses the changes introduced by the latest commit.

During the revert process, Git may open an editor for the commit message.

I used the required commit message:

```text
revert official
```

The message was kept entirely in lowercase as required by the task.

---

## Step 6: Verify the New Commit

After completing the revert, I checked the commit history again:

```bash
git log -n 3 --oneline
```

The new revert commit appeared at the top of the history.

The repository therefore contained:

```text
revert official
```

as the latest commit.

---

## Step 7: Check Repository Status

Finally, I checked the repository status:

```bash
git status
```

This confirmed the state of the repository after the revert operation.

---

# Understanding Git Revert

The command used in this challenge was:

```bash
git revert HEAD
```

`git revert` creates a **new commit** that reverses the changes introduced by an earlier commit.

It does not remove the original commit from the repository history.

For example:

```text
Before:

A → B → C
        ↑
       HEAD
```

After reverting `C`:

```text
A → B → C → D
            ↑
      revert official
```

Here:

* `C` is the original latest commit.
* `D` is the new revert commit.
* The changes introduced by `C` are reversed.
* The original commit `C` remains in Git history.

---

# Why `git revert` Was Used

The task specifically required reverting the latest commit.

`git revert` was appropriate because it preserves the existing Git history while creating a new commit that undoes the unwanted changes.

This is different from:

```bash
git reset
```

which can move the branch pointer backward and may alter the visible commit history depending on how it is used.

---

# What I Learned

From this challenge, I learned and practiced:

* How to access a Git repository on a remote Linux server.
* How to inspect Git repository status.
* How to view commit history using `git log`.
* How to identify the `HEAD` commit.
* How to revert the latest Git commit.
* How `git revert HEAD` works.
* Why reverting a commit is different from deleting it.
* How Git creates a new commit when reverting changes.
* How to use a specific commit message during a revert.
* The importance of following exact commit-message requirements.
* How to verify the repository after performing Git operations.

# Challenge Status

Day 27 — Completed Successfully 

**Latest Git commit reverted successfully while preserving the repository history.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
