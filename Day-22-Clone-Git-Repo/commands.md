# Day 22 - Commands

This file contains the commands used during **Day 22 of my 100 Days of DevOps Challenge**.

The objective was to clone the Git repository located at `/opt/media.git` into `/usr/src/kodekloudrepos` using the `natasha` user.

---

# Step 1: Access the Storage Server

Connect to the Storage Server using SSH:

```bash
ssh natasha@<storage-server-hostname>
```

---

# Step 2: Verify the Current User

Check the logged-in user:

```bash
whoami
```

Expected output:

```text
natasha
```

---

# Step 3: Navigate to the Destination Directory

Move to the required destination directory:

```bash
cd /usr/src/kodekloudrepos
```

---

# Step 4: Verify the Current Directory

Check the current working directory:

```bash
pwd
```

Expected output:

```text
/usr/src/kodekloudrepos
```

---

# Step 5: Check Existing Contents

Before cloning, check the contents of the destination directory:

```bash
ls -la
```

This helps ensure that existing files and directories are not unnecessarily modified.

---

# Step 6: Clone the Repository

Clone the Git repository from `/opt/media.git`:

```bash
git clone /opt/media.git
```

The repository will be cloned into a directory named:

```text
media
```

The resulting path is:

```text
/usr/src/kodekloudrepos/media
```

---

# Step 7: Verify the Cloned Repository

List the destination directory:

```bash
ls -la
```

Verify that the `media` directory has been created.

---

# Step 8: Enter the Repository

Navigate into the cloned repository:

```bash
cd media
```

---

# Step 9: Check Repository Contents

List the repository files:

```bash
ls -la
```

---

# Step 10: Verify Git Status

Check the status of the repository:

```bash
git status
```

This verifies that the directory is a valid Git repository and helps confirm its working-tree state.

---

# Step 11: Verify Git Remote

Check the configured remote:

```bash
git remote -v
```

The output should show the source repository:

```text
/opt/media.git
```

---

# Quick Command Summary

```bash
# Access Storage Server
ssh natasha@<storage-server-hostname>

# Verify user
whoami

# Navigate to destination
cd /usr/src/kodekloudrepos

# Verify current directory
pwd

# Check existing contents
ls -la

# Clone repository
git clone /opt/media.git

# Verify cloned repository
ls -la

# Enter repository
cd media

# Check repository contents
ls -la

# Verify Git status
git status

# Verify remote repository
git remote -v
```

---


**Day 22 completed successfully.** 
