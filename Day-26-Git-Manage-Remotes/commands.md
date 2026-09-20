# Day 26 - Git Remote Repository Management

This file contains the commands used during **Day 26 of my 100 Days of DevOps Challenge**.

The objective was to add a new Git remote, copy `/tmp/index.html` into the repository, commit it to the `master` branch, and push the changes to the new remote.

---

# Step 1: Access the Server

Connect to the required server:

```bash
ssh <username>@<server-hostname>
```

---

# Step 2: Verify the Server

```bash
hostname
```

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

# Step 4: Check Git Status

```bash
git status
```

This checks the current state of the working directory.

---

# Step 5: Check Existing Remotes

```bash
git remote -v
```

This displays the currently configured Git remotes.

---

# Step 6: Add the New Remote

Add the required remote:

```bash
git remote add dev_official /opt/xfusioncorp_official.git
```

Here:

```text
dev_official
```

is the name of the new remote.

And:

```text
/opt/xfusioncorp_official.git
```

is the remote repository location.

---

# Step 7: Verify the New Remote

```bash
git remote -v
```

Expected output should contain:

```text
dev_official    /opt/xfusioncorp_official.git (fetch)
dev_official    /opt/xfusioncorp_official.git (push)
```

---

# Step 8: Check the Current Branch

```bash
git branch
```

The required branch was:

```text
master
```

If required, switch to it:

```bash
git checkout master
```

---

# Step 9: Copy index.html

The source file was:

```text
/tmp/index.html
```

Copy it into the repository:

```bash
cp /tmp/index.html .
```

---

# Step 10: Verify the File

Check the repository contents:

```bash
ls -l
```

Then check Git status:

```bash
git status
```

The file should appear as an untracked file:

```text
index.html
```

---

# Step 11: Stage the File

Add the file to the Git staging area:

```bash
git add index.html
```

Verify the staging area:

```bash
git status
```

---

# Step 12: Commit the Changes

Create a commit:

```bash
git commit -m "Add index.html"
```

---

# Step 13: Push Master to the New Remote

Push the `master` branch to `dev_official`:

```bash
git push dev_official master
```

This pushes the local `master` branch to the remote repository:

```text
/opt/xfusioncorp_official.git
```

---

# Step 14: Final Verification

Check the repository status:

```bash
git status
```

Check the configured remotes:

```bash
git remote -v
```

Check the commit history:

```bash
git log --oneline
```

---

# Quick Command Summary

```bash
# Access server
ssh <username>@<server-hostname>

# Verify server
hostname

# Navigate to repository
cd /usr/src/kodekloudrepos/official

# Check repository status
git status

# Check existing remotes
git remote -v

# Add new remote
git remote add dev_official /opt/xfusioncorp_official.git

# Verify remote
git remote -v

# Check branch
git branch

# Switch to master if required
git checkout master

# Copy required file
cp /tmp/index.html .

# Check file/status
ls -l
git status

# Stage file
git add index.html

# Commit changes
git commit -m "Add index.html"

# Push master to new remote
git push dev_official master

# Final verification
git status
git remote -v
git log --oneline
```

---


# Important Lesson

When working with multiple Git remotes, it is important to specify the correct remote when pushing changes.

For this challenge, the important command was:

```bash
git push dev_official master
```

rather than simply:

```bash
git push
```

The final workflow was:

```text
Add Remote
    ↓
Copy File
    ↓
Stage
    ↓
Commit
    ↓
Push Master to dev_official
    ↓
Verify
```

**Day 26 completed successfully.** 
