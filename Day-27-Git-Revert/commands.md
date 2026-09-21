# Day 27 - Commands

This file contains the commands used during **Day 27 of my 100 Days of DevOps Challenge**.

The objective was to revert the latest commit (`HEAD`) in the `official` Git repository and create a new revert commit with the message:

```text
revert official
```

---

# Step 1: Access the Storage Server

Connect to the Storage Server using SSH:

```bash
ssh <username>@<storage-server-hostname>
```

---

# Step 2: Verify the Server

Check the hostname:

```bash
hostname
```

This confirms that the correct server has been accessed.

---

# Step 3: Navigate to the Repository

The repository was located at:

```text
/usr/src/kodekloudrepos/official
```

Navigate to it:

```bash
cd /usr/src/kodekloudrepos/official
```

---

# Step 4: Check Repository Status

Check the current Git status:

```bash
git status
```

This verifies the current state of the working tree before making changes.

---

# Step 5: View Commit History

Display the recent commits:

```bash
git log -n 5 --oneline
```

This helps identify:

```text
HEAD
Previous commit
Initial commit/message
```

The latest commit was the commit that needed to be reverted.

---

# Step 6: Revert the Latest Commit

Revert the current `HEAD` commit:

```bash
git revert HEAD
```

Git may open the default editor for the new commit message.

Set the commit message to:

```text
revert official
```

The message must be written in lowercase.

---

# Step 7: Verify the Commit History

After the revert is completed, check the history again:

```bash
git log -n 3 --oneline
```

The latest commit should now be the new revert commit:

```text
revert official
```

The original commit should still remain in the Git history.

---

# Step 8: Check Repository Status

Verify the repository status:

```bash
git status
```

This confirms the final state of the working tree.

---

# Quick Command Summary

```bash
# Access Storage Server
ssh <username>@<storage-server-hostname>

# Verify server
hostname

# Navigate to repository
cd /usr/src/kodekloudrepos/official

# Check repository status
git status

# View commit history
git log --oneline

# Revert latest commit
git revert HEAD

# Use this commit message
revert official

# Verify commit history
git log --oneline

# Check final repository status
git status
```

---

# Alternative Revert Command

If you already know the exact commit hash that needs to be reverted, you can also use:

```bash
git revert <commit-hash>
```

For this challenge, the latest commit was identified using:

```bash
git log 3
```

and reverted using:

```bash
git revert HEAD
```

---

# Understanding the Git History

Before the revert:

```text
Previous Commit
      ↓
Latest Commit
      ↓
     HEAD
```

After:

```text
Previous Commit
      ↓
Latest Commit
      ↓
revert official
      ↓
     HEAD
```

The original latest commit is not deleted. Instead, Git creates a new commit that reverses its changes.

---

# Important Lesson

The key command for this challenge was:

```bash
git revert HEAD
```

Unlike `git reset`, `git revert` preserves the existing commit history by creating a new commit that reverses the changes from the selected commit.

The required final commit message was:

```text
revert official
```

and it had to be written entirely in lowercase.

**Day 27 completed successfully.** 
