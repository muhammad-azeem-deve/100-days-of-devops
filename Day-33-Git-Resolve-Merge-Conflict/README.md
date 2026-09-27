# Day 33 - Fix Git Repository and Push Story Changes

## Challenge Overview

This is Day 33 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Git Repository Troubleshooting and Pushing Changes**.

Sarah and Max were working on writing stories and had already pushed their work to the repository. Max had recently made some changes, but he was facing issues while trying to push those changes to the origin repository.

The task was to access the Storage Server as the `max` user, troubleshoot the Git repository, fix the story index, and successfully push the changes to the origin repository.

### Challenge Requirement

SSH into the **Storage Server** using the `max` user and access the existing repository:

```text
/home/max/story-blog
```

The following requirements had to be completed:

* Fix the Git issues preventing Max from pushing the changes.
* Make sure `story-index.txt` contains titles for all 4 stories.
* Correct the typo:

  ```text
  The Lion and the Mooose
  ```

  to:

  ```text
  The Lion and the Mouse
  ```
* Commit the changes.
* Push the changes to the origin repository.
* Verify that the changes were successfully pushed.

---

## Objectives

The objectives of this challenge are:

* Access the Storage Server using SSH.
* Work with an existing Git repository.
* Inspect the repository status.
* Check the configured Git remote.
* Inspect the current branch and commit history.
* Fix the story index.
* Correct the spelling mistake in the story title.
* Make sure all four story titles are present.
* Stage and commit the changes.
* Troubleshoot Git push-related issues.
* Push the changes to the origin repository.
* Verify the successful push.
* Understand the basic Git troubleshooting workflow.

---

## Environment

| Item             | Details                |
| ---------------- | ---------------------- |
| Challenge        | 100 Days of DevOps     |
| Platform         | KodeKloud              |
| Day              | 33                     |
| Server           | Storage Server         |
| User             | `max`                  |
| Repository       | `story-blog`           |
| Repository Path  | `/home/max/story-blog` |
| File             | `story-index.txt`      |
| Required Stories | 4                      |
| Required Fix     | `Mooose` → `Mouse`     |
| Remote           | Origin Repository      |
| Status           | Completed              |

---

# Solution

## Step 1: SSH into the Storage Server

First, I connected to the Storage Server using the `max` user.

```bash
ssh max@ststor01
```

The password provided by the challenge was used when prompted.

---

## Step 2: Verify the Current User

After connecting to the server, I verified that I was logged in as `max`.

```bash
whoami
```

Expected output:

```text
max
```

---

## Step 3: Go to the Story Repository

The repository was located under Max's home directory.

```bash
cd /home/max/story-blog
```

I verified the current location:

```bash
pwd
```

Expected path:

```text
/home/max/story-blog
```

---

## Step 4: Check the Repository Status

Before making any changes, I checked the Git status:

```bash
git status
```

This helped me understand the current state of the repository and identify any existing changes.

---

## Step 5: Check the Git Remote

I checked the configured remote repository:

```bash
git remote -v
```

This allowed me to verify that the repository was connected to the correct origin repository.

---

## Step 6: Check the Current Branch

I checked the current Git branch:

```bash
git branch
```

The branch marked with `*` is the branch currently checked out.

I used the actual branch name shown by Git when pushing the changes.

---

## Step 7: Check the Story Index

I opened the story index:

```bash
cat story-index.txt
```

The task required the file to contain titles for **all 4 stories**.

I checked the file carefully and corrected the typo:

```text
The Lion and the Mooose
```

to:

```text
The Lion and the Mouse
```

I also made sure that all four story titles were present.

---

## Step 8: Edit the Story Index

I edited the file using:

```bash
vi story-index.txt
```

or:

```bash
nano story-index.txt
```

After making the corrections, I saved the file and exited the editor.

---

## Step 9: Check the Changes

I checked the modified files:

```bash
git status
```

Then I reviewed the exact changes:

```bash
git diff
```

This allowed me to verify that the story index contained the required corrections before committing.

---

## Step 10: Check Git History

I checked the repository history:

```bash
git log --oneline --all
```

This helped me understand the existing commits and repository state before pushing Max's changes.

---

## Step 11: Fix Git Configuration if Required

If Git required the user's identity to be configured, I configured Max's Git identity:

```bash
git config user.name "Max"
git config user.email "max@example.com"
```

The actual email should be the appropriate email configured for the repository/environment if one was provided.

The configuration can be checked using:

```bash
git config --list
```

---

## Step 12: Stage the Changes

After fixing `story-index.txt`, I staged the file:

```bash
git add story-index.txt
```

I then checked the status:

```bash
git status
```

---

## Step 13: Commit the Changes

I committed the changes with a meaningful commit message:

```bash
git commit -m "Fix story index titles"
```

This created a new commit containing the corrected story index.

---

## Step 14: Push Changes to Origin

I checked the current branch:

```bash
git branch
```

Then I pushed the changes to the origin repository.

For example, if the current branch was `master`:

```bash
git push origin master
```

If the current branch was another branch, I used that branch name instead.

---

## Step 15: Verify the Push

After pushing, I checked the repository status:

```bash
git status
```

I also checked the latest commits:

```bash
git log --oneline -5
```

The working tree should be clean after the successful commit and push.

---

## Step 16: Verify the Story Index

Finally, I checked the file again:

```bash
cat story-index.txt
```

I verified that:

* All 4 story titles were present.
* `Mooose` had been corrected to `Mouse`.
* The changes had been committed.
* The changes had been pushed to the origin repository.

---

# Git Troubleshooting Process

The important lesson from this challenge was to troubleshoot the repository instead of immediately making random changes.

The basic workflow was:

```text
SSH into Storage Server
        ↓
Access /home/max/story-blog
        ↓
Check git status
        ↓
Check git remote
        ↓
Check current branch
        ↓
Check story-index.txt
        ↓
Fix story titles
        ↓
Check git diff
        ↓
Stage changes
        ↓
Commit changes
        ↓
Push to origin
        ↓
Verify successful push
```

---

# Gitea Verification

The challenge also provided access to the **Gitea web interface**.

After pushing the changes, the repository could be checked through the Gitea UI to verify that the commit and updated `story-index.txt` were present in the remote repository.

The Gitea interface was accessed using the credentials provided by the challenge.

---


# What I Learned

From this challenge, I learned and practiced:

* How to work with an existing Git repository.
* How to troubleshoot Git repositories before making changes.
* How to check Git repository status using `git status`.
* How to check remote repositories using `git remote -v`.
* How to inspect branches using `git branch`.
* How to inspect commit history using `git log`.
* How to review modifications using `git diff`.
* How to edit and correct repository files.
* How to stage changes using `git add`.
* How to create commits using `git commit`.
* How to push changes to an origin repository.
* How Git and a Gitea web interface work together.
* The importance of verifying changes before pushing them.
* How to troubleshoot Git problems step by step.

# Challenge Status

Day 33 — Completed Successfully 

**Fixed the story index, corrected the typo, resolved the Git issues, and pushed Max's changes to the origin repository.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
