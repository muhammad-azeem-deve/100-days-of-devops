# Day 25 - Git Branching and Merging Commands

This file contains the commands used during **Day 25 of my 100 Days of DevOps Challenge**.

The objective was to create a `nautilus` branch from `master`, add `/tmp/index.html`, commit the change, merge the branch into `master`, and push both branches to the remote repository.

---

# Step 1: Access the Storage Server

Connect to the Storage Server:

```bash
ssh <username>@<storage-server-hostname>
```

Verify the server:

```bash
hostname
```

---

# Step 2: Navigate to the Repository

```bash
cd /usr/src/kodekloudrepos/news
```

---

# Step 3: Check Git Status

```bash
git status
```

---

# Step 4: Check Existing Branches

```bash
git branch
```

---

# Step 5: Switch to Master

```bash
git checkout master
```

---

# Step 6: Pull the Latest Master Changes

```bash
git pull origin master
```

This ensures that the local `master` branch is up to date before creating the new branch.

---

# Step 7: Create the Nautilus Branch

Create and switch to the new branch:

```bash
git checkout -b nautilus
```

Verify the branch:

```bash
git branch
```

Expected:

```text
* nautilus
  master
```

---

# Step 8: Copy index.html

Copy the required file from `/tmp` into the repository:

```bash
cp /tmp/index.html .
```

Verify the file:

```bash
ls -l index.html
```

---

# Step 9: Check Git Status

```bash
git status
```

The new file should appear as an untracked file.

---

# Step 10: Add the File

Stage the file:

```bash
git add index.html
```

Verify:

```bash
git status
```

---

# Step 11: Commit the File

Create a commit:

```bash
git commit -m "Add index.html"
```

---

# Step 12: Push Nautilus Branch

Push the new branch to the remote repository:

```bash
git push origin nautilus
```

---

# Step 13: Switch Back to Master

```bash
git checkout master
```

Verify:

```bash
git branch
```

---

# Step 14: Merge Nautilus into Master

Merge the `nautilus` branch:

```bash
git merge nautilus
```

---

# Step 15: Push Master

Push the updated master branch:

```bash
git push origin master
```

---

# Step 16: Verify Local Branches

```bash
git branch
```

Expected:

```text
* master
  nautilus
```

---

# Step 17: Verify Remote Branches

```bash
git branch -r
```

Expected remote branches:

```text
origin/master
origin/nautilus
```

---

# Step 18: Verify Git History

```bash
git log --oneline --all --decorate
```

This displays the commit history and branch references.

---

# Step 19: Verify index.html

Check that the file exists in the repository:

```bash
ls -l index.html
```

You can also verify the file contents:

```bash
cat index.html
```

---

# Quick Command Summary

```bash
# Access Storage Server
ssh <username>@<storage-server-hostname>

# Navigate to repository
cd /usr/src/kodekloudrepos/news

# Check repository
git status

# Switch to master
git checkout master

# Update master
git pull origin master

# Create and switch to nautilus
git checkout -b nautilus

# Copy index.html
cp /tmp/index.html .

# Check changes
git status

# Stage file
git add index.html

# Commit changes
git commit -m "Add index.html"

# Push nautilus branch
git push origin nautilus

# Switch back to master
git checkout master

# Merge nautilus into master
git merge nautilus

# Push master
git push origin master

# Verify local branches
git branch

# Verify remote branches
git branch -r

# Verify history
git log --oneline --all --decorate
```

---

# Important Git Concepts

## Create a Branch

```bash
git checkout -b nautilus
```

Creates a new branch named `nautilus` and switches to it.

---

## Stage a File

```bash
git add index.html
```

Adds the file to the staging area.

---

## Commit Changes

```bash
git commit -m "Add index.html"
```

Creates a commit containing the staged changes.

---

## Push a Branch

```bash
git push origin nautilus
```

Pushes the local `nautilus` branch to the remote repository.

---

## Merge a Branch

```bash
git checkout master
git merge nautilus
```

Switches to `master` and merges the `nautilus` branch into it.

---

## Push Master

```bash
git push origin master
```

Pushes the updated `master` branch to the remote repository.

---

# Important Lesson

This challenge helped me understand a complete Git branching workflow:

```text
Create Branch
      ↓
Make Changes
      ↓
Stage Changes
      ↓
Commit Changes
      ↓
Push Feature Branch
      ↓
Merge into Master
      ↓
Push Master
```

The most important commands from this challenge were:

```bash
git checkout -b nautilus
git add index.html
git commit -m "Add index.html"
git push origin nautilus
git checkout master
git merge nautilus
git push origin master
```

**Day 25 completed successfully.** 🚀
