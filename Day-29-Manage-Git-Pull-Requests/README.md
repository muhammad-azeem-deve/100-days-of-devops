# Day 29 - Create and Review a Pull Request in Gitea

## Challenge Overview

This is Day 29 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Git Branching, Pull Requests, Code Review, and Protected Master Branch Workflow**.

The objective was to prevent changes from being pushed directly to the `master` branch. Instead, Max's changes had to go through a **Pull Request (PR)**, be reviewed by another user, and then be merged into the `master` branch after approval.

Max had already written a story called:

> **The 🦊 Fox and Grapes 🍇**

His changes were already pushed to the remote repository on the following branch:

```text
story/fox-and-grapes
```

The task was to create a Pull Request from this branch into `master`, assign **Tom** as the reviewer, and have Tom review and merge the Pull Request.

---

## Challenge Requirement

The task required the following workflow:

1. SSH into the Storage Server using user `max`.
2. Access the already cloned Git repository.
3. Check the repository contents.
4. Check the Git commit history using `git log`.
5. Verify the author and commit information.
6. Access the Gitea web interface.
7. Create a Pull Request.
8. Set `story/fox-and-grapes` as the source branch.
9. Set `master` as the destination branch.
10. Use the PR title:
    `Added fox-and-grapes story`
11. Assign `tom` as the reviewer.
12. Log out of Gitea as Max.
13. Log in to Gitea as Tom.
14. Open the Pull Request.
15. Review and approve the changes.
16. Merge the Pull Request into `master`.

---

## Objectives

The objectives of this challenge are:

* Understand Git branching workflows.
* Inspect an existing Git repository.
* Verify commit history using `git log`.
* Verify commit author and commit message.
* Understand why changes should not always be pushed directly to `master`.
* Create a Pull Request using Gitea.
* Configure source and destination branches.
* Assign a reviewer to a Pull Request.
* Review and approve a Pull Request.
* Merge approved changes into the `master` branch.
* Understand a basic code review workflow.

---

## Environment

| Item               | Details                      |
| ------------------ | ---------------------------- |
| Challenge          | 100 Days of DevOps           |
| Platform           | KodeKloud                    |
| Day                | 29                           |
| Git Platform       | Gitea                        |
| User               | `max`                        |
| Reviewer           | `tom`                        |
| Repository         | Existing cloned repository   |
| Source Branch      | `story/fox-and-grapes`       |
| Destination Branch | `master`                     |
| PR Title           | `Added fox-and-grapes story` |
| Workflow           | Pull Request + Code Review   |
| Status             | Completed                    |

---

# Solution

## Step 1: SSH into the Storage Server

First, I connected to the Storage Server using the `max` user.

```bash
ssh max@<storage-server>
```

Password:

```text
Max_pass123
```

After logging in, I accessed the repository that was already cloned under Max's home directory.

---

## Step 2: Check the Repository

I entered the cloned repository and checked its contents:

```bash
cd <repository-directory>
```

Then I listed the repository files:

```bash
ls
```

This allowed me to confirm that the repository was available and that the story files were present.

---

## Step 3: Check Git Status

I checked the current Git status:

```bash
git status
```

This helped confirm the current branch and whether there were any uncommitted changes.

---

## Step 4: Check Commit History

The task required confirming Max's story and the history of commits.

I used:

```bash
git log
```

The commit history allowed me to verify information such as:

* Commit author
* Commit date
* Commit message
* Commit history
* Branch changes

I verified that Max's story had already been committed and pushed to the remote repository.

---

## Step 5: Verify the Remote Repository

I checked the configured Git remote:

```bash
git remote -v
```

This confirmed the remote repository to which the local repository was connected.

---

# Creating the Pull Request

## Step 6: Open Gitea

I opened the **Gitea web interface** provided by the KodeKloud lab.

I logged into the Git Portal using Max's credentials:

```text
Username: max
Password: Max_pass123
```

---

## Step 7: Create a Pull Request

I created a new Pull Request from:

```text
story/fox-and-grapes
```

into:

```text
master
```

The Pull Request title was:

```text
Added fox-and-grapes story
```

The branch configuration was:

```text
Source:      story/fox-and-grapes
Destination: master
```

The Pull Request was then created successfully.

---

# Assigning the Reviewer

## Step 8: Add Tom as Reviewer

After creating the Pull Request, I opened the newly created PR.

From the Pull Request page, I selected the **Reviewers** option and added:

```text
tom
```

as the reviewer.

This ensured that Max's changes would be reviewed before being merged into `master`.

---

# Reviewing the Pull Request

## Step 9: Log Out as Max

After assigning Tom as the reviewer, I logged out of the Gitea portal as Max.

---

## Step 10: Login as Tom

I logged into the Gitea portal using Tom's credentials:

```text
Username: tom
Password: Tom_pass123
```

---

## Step 11: Open the Pull Request

As Tom, I opened the Pull Request:

```text
Added fox-and-grapes story
```

I reviewed the changes made in:

```text
story/fox-and-grapes
```

before merging them into:

```text
master
```

---

## Step 12: Approve and Merge

After reviewing the Pull Request, I approved the changes as Tom.

The Pull Request was then merged into:

```text
master
```

The merge was completed successfully.

---

# Git Workflow

The workflow used in this challenge was:

```text
Max creates/updates story
          |
          v
story/fox-and-grapes
          |
          v
Push changes to remote
          |
          v
Create Pull Request
          |
          v
Source: story/fox-and-grapes
Destination: master
          |
          v
Assign Tom as Reviewer
          |
          v
Tom reviews changes
          |
          v
Tom approves PR
          |
          v
Merge PR
          |
          v
master
```

---

# Why Use a Pull Request?

The purpose of this workflow is to avoid allowing everyone to directly push changes to the final branch.

Instead of:

```text
Developer → Direct Push → master
```

the workflow becomes:

```text
Developer
    ↓
Feature/Story Branch
    ↓
Pull Request
    ↓
Code Review
    ↓
Approval
    ↓
master
```

This provides an opportunity to review changes before they become part of the main codebase.

---

# What I Learned

From this challenge, I learned and practiced:

* How to inspect an existing Git repository.
* How to use `git status`.
* How to inspect Git commit history using `git log`.
* How to verify Git remote repositories.
* How Git branches are used to isolate changes.
* How to create a Pull Request in Gitea.
* How to configure source and destination branches.
* How to assign a reviewer to a Pull Request.
* How code review works before merging changes.
* How to approve a Pull Request.
* How to merge an approved Pull Request into `master`.
* Why Pull Requests are useful for maintaining code quality and controlling changes to important branches.

# Challenge Status

Day 29 — Completed Successfully 

**Created the Pull Request, assigned Tom as reviewer, reviewed and approved the changes, and successfully merged the story into the `master` branch.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
