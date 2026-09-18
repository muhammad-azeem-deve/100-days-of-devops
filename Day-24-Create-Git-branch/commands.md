# Day 24 - Commands

This file contains the commands used during **Day 24 of my 100 Days of DevOps Challenge**.

The objective was to create a new Git branch named `xfusioncorp_official` from the `master` branch in the repository located at:

```text
/usr/src/kodekloudrepos/official
```

No code changes were required.

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

This confirms that I am working on the correct server.

---

# Step 3: Navigate to the Repository

Move to the required Git repository:

```bash
cd /usr/src/kodekloudrepos/official
```

---

# Step 4: Check Repository Status

Check the current Git status:

```bash
git status
```

This helps verify the current branch and repository state before creating the new branch.

---

# Step 5: List Existing Branches

List the available branches:

```bash
git branch
```

The repository should contain the `master` branch.

---

# Step 6: Switch to Master

Switch to the `master` branch:

```bash
git checkout master
```

Verify the active branch:

```bash
git branch
```

Expected output:

```text
* master
```

---

# Step 7: Create the New Branch

Create the required branch from `master`:

```bash
git checkout -b xfusioncorp_official
```

This command:

1. Creates the `xfusioncorp_official` branch.
2. Switches the working directory to the new branch.

---

# Step 8: Verify the New Branch

List the branches again:

```bash
git branch
```

Expected output:

```text
* xfusioncorp_official
  master
```

The `*` indicates the currently active branch.

---

# Step 9: Verify Repository Status

Check the repository status:

```bash
git status
```

This confirms that the current branch is:

```text
xfusioncorp_official
```

and that no code changes were made.

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

# List branches
git branch

# Switch to master
git checkout master

# Create and switch to new branch
git checkout -b xfusioncorp_official

# Verify branch
git branch

# Verify repository status
git status
```

---

# Alternative Git Command

The branch can also be created using:

```bash
git switch -c xfusioncorp_official
```

However, the command used for this challenge was:

```bash
git checkout -b xfusioncorp_official
```

---

# Important Lesson

Before creating a new branch, it is important to make sure that the branch is created from the correct base branch.

For this challenge, the required sequence was:

```text
master
   ↓
git checkout -b xfusioncorp_official
   ↓
xfusioncorp_official
```

The key command was:

```bash
git checkout -b xfusioncorp_official
```

No application code was modified because the task only required creating the new branch.

**Day 24 completed successfully.** 
