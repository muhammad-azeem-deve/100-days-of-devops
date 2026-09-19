# Day 25 - Git Branching and Merging

## Challenge Overview

This is Day 25 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Git Branching, Committing, Merging, and Pushing Changes to a Remote Repository**.

The Nautilus application development team was working on a Git repository located at:

```text
/opt/news.git
```

The repository was already cloned on the **Storage Server** at:

```text
/usr/src/kodekloudrepos/news
```

The task was to create a new branch from `master`, copy a required `index.html` file into the repository, commit the changes to the new branch, merge the branch back into `master`, and finally push both branches to the remote origin.

### Challenge Requirement

The requirements were:

1. Access the **Storage Server**.
2. Navigate to `/usr/src/kodekloudrepos/news`.
3. Create a new branch named `nautilus` from `master`.
4. Copy `/tmp/index.html` into the repository.
5. Add the file to Git.
6. Commit the file in the `nautilus` branch.
7. Merge the `nautilus` branch back into `master`.
8. Push both `master` and `nautilus` branches to the remote `origin`.

---

## Objectives

The objectives of this challenge are:

* Work with an existing Git repository.
* Understand Git branches.
* Create a new branch from `master`.
* Copy files into a Git repository.
* Stage and commit changes.
* Switch between Git branches.
* Merge one branch into another.
* Push multiple branches to a remote repository.
* Verify the repository history and branch status.

---

## Environment

| Item              | Details                        |
| ----------------- | ------------------------------ |
| Challenge         | 100 Days of DevOps             |
| Platform          | KodeKloud                      |
| Day               | 25                             |
| Server            | Storage Server                 |
| Repository        | `/usr/src/kodekloudrepos/news` |
| Remote Repository | `/opt/news.git`                |
| Source File       | `/tmp/index.html`              |
| New Branch        | `nautilus`                     |
| Base Branch       | `master`                       |
| Remote            | `origin`                       |
| Status            | Completed                      |

---

# Solution

## Step 1: Access the Storage Server

First, I accessed the **Storage Server** using SSH.

The SSH command format was:

```bash
ssh <username>@<storage-server-hostname>
```

After connecting, I verified that I was working on the correct server:

```bash
hostname
```

---

## Step 2: Navigate to the Repository

The repository was located at:

```text
/usr/src/kodekloudrepos/news
```

I navigated to the repository:

```bash
cd /usr/src/kodekloudrepos/news
```

---

## Step 3: Check the Current Git Branch

Before creating the new branch, I checked the current branch:

```bash
git branch
```

I made sure that the repository was on the `master` branch.

I then switched to `master` if required:

```bash
git checkout master
```

---

## Step 4: Update the Master Branch

Before creating the new branch, I pulled the latest changes from the remote repository:

```bash
git pull origin master
```

This ensured that the new branch was created from the latest `master` branch.

---

## Step 5: Create the Nautilus Branch

I created the new branch named `nautilus` from `master`:

```bash
git checkout -b nautilus
```

This command created the branch and switched to it.

I verified the current branch using:

```bash
git branch
```

The output should show:

```text
* nautilus
  master
```

---

## Step 6: Copy the index.html File

The required file was located on the Storage Server at:

```text
/tmp/index.html
```

I copied it into the Git repository:

```bash
cp /tmp/index.html .
```

I then checked the repository contents:

```bash
ls -l
```

---

## Step 7: Check Git Status

Before adding the file, I checked the Git working tree:

```bash
git status
```

The `index.html` file should appear as an untracked file.

---

## Step 8: Add the File to Git

I staged the new file:

```bash
git add index.html
```

Then I verified the staged changes:

```bash
git status
```

---

## Step 9: Commit the Changes

I committed the file to the `nautilus` branch:

```bash
git commit -m "Add index.html"
```

This created a commit containing the new `index.html` file.

---

## Step 10: Push the Nautilus Branch

After committing the changes, I pushed the `nautilus` branch to the remote repository:

```bash
git push origin nautilus
```

This created/updated the `nautilus` branch on the remote repository.

---

## Step 11: Switch Back to Master

After completing the changes on the `nautilus` branch, I switched back to `master`:

```bash
git checkout master
```

I verified the active branch:

```bash
git branch
```

---

## Step 12: Merge Nautilus into Master

I merged the `nautilus` branch into `master`:

```bash
git merge nautilus
```

This brought the `index.html` changes from the `nautilus` branch into `master`.

---

## Step 13: Push Master to Origin

Finally, I pushed the updated `master` branch to the remote repository:

```bash
git push origin master
```

Now both branches were available on the remote repository.

---

## Step 14: Verify Branches

I verified the local branches:

```bash
git branch
```

Expected branches:

```text
* master
  nautilus
```

I also checked the remote branches:

```bash
git branch -r
```

The remote branches should include:

```text
origin/master
origin/nautilus
```

---

## Step 15: Verify Git History

Finally, I checked the commit history:

```bash
git log --oneline --all --decorate
```

This helped verify that the `index.html` commit existed and that the changes had been merged into `master`.

---


# What I Learned

From this challenge, I learned and practiced:

* How to work with an existing Git repository.
* How to create a new Git branch from `master`.
* How to switch between branches.
* How to copy files into a Git repository.
* How to check repository changes using `git status`.
* How to stage files using `git add`.
* How to create commits using `git commit`.
* How to push a branch to a remote repository.
* How to merge a feature branch into `master`.
* How to push the updated `master` branch.
* How to verify local and remote branches.
* How Git branching can be used to develop changes separately before merging them into the main branch.

# Challenge Status

Day 25 — Completed Successfully 

**Created the `nautilus` branch, added `index.html`, committed and pushed the branch, merged it into `master`, and pushed the updated master branch to the remote repository.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
