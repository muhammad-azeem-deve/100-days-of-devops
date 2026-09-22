# Day 28 - Commands

This file contains the commands used during **Day 28 of my 100 Days of DevOps Challenge**.

The objective was to merge only the commit with the message **`Update info.txt`** from the `feature` branch into the `master` branch and push the changes.

---

# Step 1: Access the Storage Server

Connect to the Storage Server:

```bash
ssh <username>@<storage-server>
```

---

# Step 2: Verify the Server

Check the hostname:

```bash
hostname
```

---

# Step 3: Navigate to the Repository

Move to the cloned repository:

```bash
cd /usr/src/kodekloudrepos
```

---

# Step 4: Check Git Status

```bash
git status
```

This verifies the current branch and working tree state.

---

# Step 5: Check Available Branches

List local branches:

```bash
git branch
```

List local and remote branches:

```bash
git branch -a
```

Expected branches include:

```text
master
feature
```

---

# Step 6: Find the Required Commit

View the commit history:

```bash
git log --oneline --all
```

Find the commit with the message:

```text
Update info.txt
```

Example:

```text
abc1234 Update info.txt
```

The actual commit ID must be taken from the repository.

---

# Step 7: Switch to Master

Switch to the `master` branch:

```bash
git checkout master
```

Verify the current branch:

```bash
git branch
```

Expected:

```text
* master
  feature
```

---

# Step 8: Cherry-Pick the Commit

Apply only the required commit:

```bash
git cherry-pick <commit-id>
```

Example:

```bash
git cherry-pick abc1234
```

Replace `abc1234` with the actual commit ID associated with:

```text
Update info.txt
```

---

# Step 9: Check Git Status

After cherry-picking:

```bash
git status
```

This verifies that the working tree is in the expected state.

---

# Step 10: Verify Commit History

Check the latest commits:

```bash
git log --oneline -5
```

The cherry-picked commit should now be present in the `master` branch history.

---

# Step 11: Check the Commit Details

To inspect the commit:

```bash
git show <commit-id>
```

For example:

```bash
git show abc1234
```

---

# Step 12: Verify info.txt

Since the required commit was related to `info.txt`, check the file:

```bash
cat info.txt
```

---

# Step 13: Push the Changes

Push the updated `master` branch:

```bash
git push origin master
```

This sends the changes to the remote repository.

---

# Quick Command Summary

```bash
# Access Storage Server
ssh <username>@<storage-server>

# Navigate to repository
cd /usr/src/kodekloudrepos

# Check status
git status

# List branches
git branch

# List all branches
git branch -a

# Find required commit
git log --oneline --all

# Switch to master
git checkout master

# Cherry-pick required commit
git cherry-pick <commit-id>

# Verify status
git status

# Verify recent commits
git log --oneline -5

# View commit details
git show <commit-id>

# Verify info.txt
cat info.txt

# Push changes
git push origin master
```

---

# Important Git Concept

The main command used in this challenge was:

```bash
git cherry-pick <commit-id>
```

`git cherry-pick` applies the changes introduced by a specific commit to the current branch.

In this challenge:

```text
feature
   |
   |--- Update info.txt
   |
   ↓
cherry-pick
   |
   ↓
master
```

This allowed the required commit to be moved to `master` without merging the developer's entire unfinished `feature` branch.

---

# Important Lesson

Before cherry-picking a commit, always:

1. Check the current branch.
2. Find the correct commit ID.
3. Verify the commit message.
4. Switch to the target branch.
5. Cherry-pick the specific commit.
6. Check the resulting Git history.
7. Verify the affected files.
8. Push the changes.

The key commands for this challenge were:

```bash
git log --oneline --all
git checkout master
git cherry-pick <commit-id>
git push origin master
```

**Day 28 completed successfully.** 
