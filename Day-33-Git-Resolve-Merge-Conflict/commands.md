# Day 33 - Commands

This file contains the commands used during **Day 33 of my 100 Days of DevOps Challenge by KodeKloud**.

The objective was to troubleshoot the `story-blog` Git repository, fix `story-index.txt`, and push Max's changes to the origin repository.

---

# Step 1: SSH into Storage Server

Connect to the Storage Server as `max`:

```bash
ssh max@ststor01
```

Enter the password provided by the challenge when prompted.

---

# Step 2: Verify the Current User

```bash
whoami
```

Expected output:

```text
max
```

---

# Step 3: Go to the Repository

```bash
cd /home/max/story-blog
```

Verify the current directory:

```bash
pwd
```

Expected:

```text
/home/max/story-blog
```

---

# Step 4: Check Git Status

```bash
git status
```

This shows:

* Current branch
* Modified files
* Untracked files
* Staged changes

---

# Step 5: Check Git Remote

```bash
git remote -v
```

This displays the configured `origin` repository.

---

# Step 6: Check Current Branch

```bash
git branch
```

The branch containing `*` is the current branch.

For example:

```text
* master
```

---

# Step 7: Check Repository Files

```bash
ls -la
```

---

# Step 8: View Story Index

```bash
cat story-index.txt
```

The file must contain titles for all four stories.

The typo:

```text
The Lion and the Mooose
```

must be corrected to:

```text
The Lion and the Mouse
```

---

# Step 9: Edit Story Index

Open the file:

```bash
vi story-index.txt
```

or:

```bash
nano story-index.txt
```

Make sure:

* All 4 story titles are present.
* `Mooose` is changed to `Mouse`.

---

# Step 10: Check File Changes

```bash
git status
```

Then review the exact changes:

```bash
git diff
```

---

# Step 11: Check Git History

```bash
git log --oneline --all
```

For a shorter recent history:

```bash
git log --oneline -5
```

---

# Step 12: Check Git Configuration

Check the configured Git username:

```bash
git config user.name
```

Check the configured email:

```bash
git config user.email
```

If required, configure the identity:

```bash
git config user.name "Max"
git config user.email "max@example.com"
```

---

# Step 13: Stage the Changes

Stage the corrected file:

```bash
git add story-index.txt
```

Verify:

```bash
git status
```

---

# Step 14: Commit the Changes

Create a commit:

```bash
git commit -m "Fix story index titles"
```

---

# Step 15: Check Current Branch Again

```bash
git branch
```

Use the branch marked with `*` when pushing.

---

# Step 16: Push Changes to Origin

If the current branch is `master`:

```bash
git push origin master
```

If the current branch is different, replace `master` with the actual branch name.

For example:

```bash
git push origin <current-branch>
```

---

# Step 17: Verify Repository Status

After pushing:

```bash
git status
```

A successful push should leave the working tree clean if there are no other changes.

---

# Step 18: Verify Latest Commit

```bash
git log --oneline -5
```

The latest commit should contain the changes made to the story index.

---

# Step 19: Verify Story Index Again

```bash
cat story-index.txt
```

Verify:

```text
All 4 story titles are present
```

and:

```text
The Lion and the Mouse
```

is correctly spelled.

---

# Useful Git Troubleshooting Commands

## Check Remote

```bash
git remote -v
```

## Check Branch

```bash
git branch
```

## Check Status

```bash
git status
```

## Check Differences

```bash
git diff
```

## Check Commit History

```bash
git log --oneline --all
```

## Check Git User

```bash
git config user.name
```

## Check Git Email

```bash
git config user.email
```

---

# Complete Command Sequence

```bash
# Connect to Storage Server
ssh max@ststor01

# Verify user
whoami

# Go to repository
cd /home/max/story-blog

# Verify location
pwd

# Check repository status
git status

# Check remote
git remote -v

# Check branch
git branch

# View files
ls -la

# View story index
cat story-index.txt

# Edit story index
vi story-index.txt

# Review changes
git diff

# Check Git history
git log --oneline --all

# Configure Git identity if required
git config user.name "Max"
git config user.email "max@example.com"

# Stage changes
git add story-index.txt

# Check staged changes
git status

# Commit changes
git commit -m "Fix story index titles"

# Check current branch
git branch

# Push changes
git push origin <current-branch>

# Verify final status
git status

# Verify latest commit
git log --oneline -5

# Verify story index
cat story-index.txt
```

---

# Important Lesson

When troubleshooting a Git push problem, don't immediately delete the repository or clone it again.

First inspect the repository:

```bash
git status
git remote -v
git branch
git log --oneline --all
git diff
```

Then fix the actual problem, make the required file changes, commit them, and push.

The main Git workflow for this challenge was:

```text
Check
  ↓
Fix
  ↓
Stage
  ↓
Commit
  ↓
Push
  ↓
Verify
```

The required file was:

```text
story-index.txt
```

The required correction was:

```text
The Lion and the Mooose
```

to:

```text
The Lion and the Mouse
```

The final goal was to have all **4 story titles** correctly listed and successfully push Max's changes to the origin repository.

**Day 33 completed successfully.** 
