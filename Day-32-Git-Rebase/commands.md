# Day 32 - Commands

This file contains the commands used during **Day 32 of my 100 Days of DevOps Challenge**.

The objective was to rebase the developer's feature branch with the latest `master` branch without losing feature-branch changes or creating a merge commit.

---

# Step 1: Access the Storage Server

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
cd /usr/src/kodekloudrepos/games
```

---

# Step 3: Check Repository Status

```bash
git status
```

---

# Step 4: Check Available Branches

Check local branches:

```bash
git branch
```

Check local and remote branches:

```bash
git branch -a
```

Identify the developer's feature branch from the output.

---


# Step 6: Update Master

Pull the latest changes:

```bash
git pull origin master
```

---

# Step 7: Switch to Feature Branch

Replace `<feature-branch>` with the actual branch name:

```bash
git checkout <feature-branch>
```

Verify:

```bash
git branch
```

---

# Step 8: Rebase Feature Branch

Rebase the feature branch onto the updated master:

```bash
git rebase master
```

This applies the feature branch commits on top of the latest `master` commits.

---

# Step 9: Check for Conflicts

If Git reports conflicts:

```bash
git status
```

Open the conflicting files and resolve the conflicts.

After resolving:

```bash
git add <file>
```

Continue the rebase:

```bash
git rebase --continue
```

Repeat these steps if there are additional conflicts.

---

# Step 10: Abort Rebase if Necessary

If the rebase needs to be cancelled:

```bash
git rebase --abort
```

This returns the feature branch to its state before the rebase started.

---

# Step 11: Verify Git History

After the rebase is completed:

```bash
git log --oneline --graph --all
```

Also check the repository status:

```bash
git status
```

---

# Step 12: Push the Rebasing Changes

Because rebase rewrites the feature branch history, use:

```bash
git push --force-with-lease origin <feature-branch>
```

Replace `<feature-branch>` with the actual feature branch name.

---

# Quick Command Summary

```bash
# Access Storage Server
ssh <username>@<storage-server-hostname>

# Navigate to repository
cd /usr/src/kodekloudrepos/games

# Check status
git status

# Check branches
git branch -a

# Switch to master
git checkout master

# Update master
git pull origin master

# Switch to feature branch
git checkout <feature-branch>

# Rebase feature branch onto master
git rebase master

# Check status
git status

# If conflicts occur, resolve them and stage the files
git add <file>

# Continue rebase
git rebase --continue

# Verify history
git log --oneline --graph --all

# Push rebased branch
git push --force-with-lease origin <feature-branch>
```

---

# Rebase vs Merge

## Merge

```bash
git checkout <feature-branch>
git merge master
```

A merge can create an additional merge commit.

---

## Rebase

```bash
git checkout <feature-branch>
git rebase master
```

Rebase reapplies the feature commits on top of the latest master history.

For this challenge, **rebase was required** because the task specifically required updating the feature branch without adding a merge commit.

---



# Important Lesson

The key concept in this challenge was:

```bash
git rebase master
```

Rebase allows the feature branch to be updated with the latest `master` changes while keeping the feature commits and avoiding a merge commit.

Because rebasing changes commit history, the final push may require:

```bash
git push --force-with-lease origin <feature-branch>
```

**Day 32 completed successfully.** 
