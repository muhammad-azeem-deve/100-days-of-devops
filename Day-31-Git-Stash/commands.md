# Day 31 - Commands

This file contains the commands used during **Day 31 of my 100 Days of DevOps Challenge**.

The objective was to restore the Git stash identified as `stash@{1}` from the `beta` repository, commit the restored changes, and push them to the `origin` remote.

---

# Step 1: Access Storage Server

Connect to the Storage Server:

```bash
ssh <username>@<storage-server-hostname>
```

Example:

```bash
ssh natasha@ststor01
```

---

# Step 2: Navigate to the Repository

```bash
cd /usr/src/kodekloudrepos/beta
```

---

# Step 3: Check Repository Status

```bash
git status
```

---

# Step 4: Check Remote Repository

```bash
git remote -v
```

This verifies the configured remote repository and confirms that `origin` is available.

---

# Step 5: Check Current Branch

```bash
git branch --show-current
```

This displays the branch on which the changes will be committed and pushed.

---

# Step 6: List Git Stashes

```bash
git stash list
```

Look for the required stash:

```text
stash@{1}
```

---

# Step 7: Inspect the Required Stash

Check a summary of the changes:

```bash
git stash show stash@{1}
```

For a detailed view:

```bash
git stash show -p stash@{1}
```

---

# Step 8: Restore the Required Stash

Restore `stash@{1}`:

```bash
git stash pop stash@{1}
```

This applies the changes from the specified stash to the working directory.

---

# Step 9: Check Restored Changes

Check the repository status:

```bash
git status
```

Check the actual changes:

```bash
git diff
```

---

# Step 10: Stage the Changes

Stage all restored changes:

```bash
git add .
```

Verify the staging area:

```bash
git status
```

---

# Step 11: Commit the Changes

Create a commit:

```bash
git commit -m "Restore stashed changes"
```

---

# Step 12: Push Changes to Origin

First, check the current branch:

```bash
git branch --show-current
```

Then push the changes:

```bash
git push origin <branch-name>
```

For example:

```bash
git push origin master
```

or:

```bash
git push origin main
```

---

# Step 13: Verify the Latest Commit

Check the latest commit:

```bash
git log -1 --oneline
```

---

# Step 14: Verify Repository Status

Finally, check:

```bash
git status
```

A clean working tree after a successful commit and push should show that there are no remaining local changes.

---

# Quick Command Summary

```bash
# Access Storage Server
ssh <username>@<storage-server-hostname>

# Navigate to repository
cd /usr/src/kodekloudrepos/beta

# Check repository status
git status

# Check remote
git remote -v

# Check current branch
git branch --show-current

# List stashes
git stash list

# Inspect stash@{1}
git stash show stash@{1}

# Inspect detailed stash changes
git stash show -p stash@{1}

# Restore stash@{1}
git stash pop stash@{1}

# Check restored changes
git status
git diff

# Stage changes
git add .

# Commit changes
git commit -m "Restore stashed changes"

# Push to origin
git push origin <branch-name>

# Verify latest commit
git log -1 --oneline

# Final status
git status
```

---

# Understanding the Main Git Commands

## List Stashes

```bash
git stash list
```

Displays all stashes stored in the repository.

Example:

```text
stash@{0}
stash@{1}
stash@{2}
```

---

## Inspect a Stash

```bash
git stash show -p stash@{1}
```

Displays the detailed changes stored in `stash@{1}`.

---

## Restore a Stash

```bash
git stash pop stash@{1}
```

Applies the changes from the specified stash to the working directory.

`pop` also attempts to remove the stash after it has been successfully applied.

---

## Stage Changes

```bash
git add .
```

Stages the restored changes for the next commit.

---

## Commit Changes

```bash
git commit -m "Restore stashed changes"
```

Creates a new Git commit containing the staged changes.

---

## Push Changes

```bash
git push origin <branch-name>
```

Pushes the local commit to the remote repository.

---

# Important Lesson

Git stash is useful when developers need to temporarily save unfinished work without committing it.

In this challenge, the important workflow was:

```text
git stash list
        ↓
Find stash@{1}
        ↓
git stash show -p stash@{1}
        ↓
git stash pop stash@{1}
        ↓
git add .
        ↓
git commit
        ↓
git push origin
```

The key command for this challenge was:

```bash
git stash pop stash@{1}
```

This restored the specific stashed changes requested by the task.

**Day 31 completed successfully.** 
