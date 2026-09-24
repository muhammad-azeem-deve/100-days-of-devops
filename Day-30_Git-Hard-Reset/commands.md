# Day 30 - Commands

This file contains the commands used during **Day 30 of my 100 Days of DevOps Challenge**.

The objective was to reset the Git repository history so that only the following two commits remained:

```text
initial commit
add data.txt file
```

The repository was located at:

```text
/usr/src/kodekloudrepos/blog
```

---

# Step 1: Access the Storage Server

Connect to the Storage Server using SSH:

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
cd /usr/src/kodekloudrepos/blog
```

---

# Step 3: Check Git Status

```bash
git status
```

This checks the current working tree and repository state.

---

# Step 4: View Git Commit History

Display the commit history:

```bash
git log --oneline
```

For a more complete view:

```bash
git log --oneline --all
```

The objective was to locate the commit with the message:

```text
add data.txt file
```

---

# Step 5: Find the Required Commit

Use the Git history to identify the commit ID associated with:

```text
add data.txt file
```

For example:

```text
abc1234 add data.txt file
```

The actual commit ID will be different in the KodeKloud environment.

---

# Step 6: Reset to the Required Commit

After identifying the correct commit:

```bash
git reset --hard <commit-id>
```

Example:

```bash
git reset --hard abc1234
```

The `--hard` option resets:

```text
HEAD
Index
Working Tree
```

to the selected commit.

---

# Step 7: Verify Commit History

Check the history again:

```bash
git log --oneline
```

Expected result:

```text
<commit-id> add data.txt file
<commit-id> initial commit
```

Only these two commits should remain on the current branch.

---

# Step 8: Check Working Tree

```bash
git status
```

The working tree should be clean.

Expected output will indicate that there are no changes to commit.

---

# Step 9: Verify Latest Commit

Check the latest commit:

```bash
git log -1 --oneline
```

Expected result:

```text
<commit-id> add data.txt file
```

---

# Step 10: Verify HEAD

Check the current `HEAD`:

```bash
git rev-parse HEAD
```

The output should match the commit ID of:

```text
add data.txt file
```

---

# Step 11: Check Current Branch

```bash
git branch --show-current
```

This shows the branch currently being used.

---

# Step 12: Push the Updated History

Because the local Git history was rewritten, push the changes to the remote repository:

```bash
git push --force
```

If the remote and branch need to be specified explicitly:

```bash
git push --force origin <branch-name>
```

For example:

```bash
git push --force origin master
```

---

# Quick Command Summary

```bash
# Access Storage Server
ssh <username>@<storage-server-hostname>

# Navigate to repository
cd /usr/src/kodekloudrepos/blog

# Check repository status
git status

# View commit history
git log --oneline

# Find all commits
git log --oneline --all

# Reset to the required commit
git reset --hard <commit-id>

# Verify history
git log --oneline

# Verify working tree
git status

# Verify latest commit
git log -1 --oneline

# Verify HEAD
git rev-parse HEAD

# Check current branch
git branch --show-current

# Push rewritten history
git push --force
```

---

# Understanding git reset --hard

The main command used in this challenge was:

```bash
git reset --hard <commit-id>
```

The command moves the current branch and `HEAD` to the selected commit.

The `--hard` option also updates the working tree and staging area.

```text
Before:

initial commit
      ↓
add data.txt file
      ↓
test commit
      ↓
another test commit
      ↓
HEAD


After:

initial commit
      ↓
add data.txt file
      ↓
HEAD
```

The unwanted commits after `add data.txt file` are no longer part of the current branch history.

---

# Final Verification

Run:

```bash
git log --oneline
```

The final history should contain:

```text
<commit-id> add data.txt file
<commit-id> initial commit
```

Then verify:

```bash
git status
```

and:

```bash
git log -1 --oneline
```

The latest commit must be:

```text
add data.txt file
```

Finally, push the updated history:

```bash
git push --force
```

---

# Important Lesson

`git reset --hard` is a powerful and potentially destructive command. Before using it, always verify the target commit carefully.

In this challenge, the target was specifically the commit with the message:

```text
add data.txt file
```

After resetting, the repository needed to contain only:

```text
initial commit
add data.txt file
```

Because the commit history was rewritten, a force push was required to update the remote repository.

**Day 30 completed successfully.** 
