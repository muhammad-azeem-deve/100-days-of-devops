# Day 29 - Commands

This file contains the Git and Linux commands used during **Day 29 of my 100 Days of DevOps Challenge**.

The main Pull Request, review, approval, and merge operations were performed through the **Gitea web interface**.

---

# Step 1: SSH into Storage Server

Connect to the Storage Server as `max`:

```bash
ssh max@<storage-server>
```

Password:

```text
Max_pass123
```

---

# Step 2: Navigate to Repository

Move into the already cloned repository:

```bash
cd <repository-directory>
```

---

# Step 3: List Repository Contents

```bash
ls
```

This allows you to inspect the files available in the repository.

---

# Step 4: Check Git Status

```bash
git status
```

This displays:

* Current branch
* Modified files
* Untracked files
* Staged changes

---

# Step 5: Check Git Branches

List local branches:

```bash
git branch
```

List all local and remote branches:

```bash
git branch -a
```

The story branch should be visible:

```text
story/fox-and-grapes
```

---

# Step 6: Check Git Commit History

View the complete commit history:

```bash
git log
```

This allows you to verify:

* Author
* Commit message
* Commit date
* Commit history

---

# Step 7: View Compact Commit History

For a shorter version of the commit history:

```bash
git log --oneline
```

---

# Step 8: View Detailed Commit Information

To inspect a particular commit:

```bash
git show <commit-id>
```

This displays the commit information and the changes introduced by that commit.

---

# Step 9: Check Remote Repository

Verify the configured remote repository:

```bash
git remote -v
```

This displays the fetch and push URLs configured for the repository.

---

# Step 10: Check Current Branch

```bash
git branch --show-current
```

This shows the branch currently checked out.

---

# Gitea Pull Request Configuration

The following configuration was used when creating the Pull Request through the Gitea UI:

```text
PR Title:
Added fox-and-grapes story
```

Source branch:

```text
story/fox-and-grapes
```

Destination branch:

```text
master
```

Reviewer:

```text
tom
```

---

# Pull Request Workflow

The Pull Request workflow was:

```text
story/fox-and-grapes
        |
        v
Create Pull Request
        |
        v
master
        |
        v
Assign tom as Reviewer
        |
        v
Tom Reviews Changes
        |
        v
Tom Approves
        |
        v
Merge Pull Request
```

---

# Login Information Used in the Lab

## Max

```text
Username: max
Password: Max_pass123
```

Max was used to:

* Access the Storage Server.
* Inspect the repository.
* Verify the Git history.
* Create the Pull Request.
* Assign Tom as reviewer.

---

## Tom

```text
Username: tom
Password: Tom_pass123
```

Tom was used to:

* Log into the Gitea portal.
* Open the Pull Request.
* Review the changes.
* Approve the Pull Request.
* Merge the Pull Request.

---

# Useful Git Commands

## Check Repository Status

```bash
git status
```

## List Branches

```bash
git branch -a
```

## Check Current Branch

```bash
git branch --show-current
```

## View Commit History

```bash
git log
```

## View Compact History

```bash
git log --oneline
```

## View Commit Details

```bash
git show <commit-id>
```

## Check Remote Repository

```bash
git remote -v
```

---

# Complete Command Summary

```bash
# SSH into Storage Server
ssh max@<storage-server>

# Navigate to repository
cd <repository-directory>

# List repository files
ls

# Check repository status
git status

# List branches
git branch -a

# Check current branch
git branch --show-current

# View commit history
git log

# View compact commit history
git log --oneline

# View a specific commit
git show <commit-id>

# Check remote repository
git remote -v
```

---

# Important Note

The following operations were completed through the **Gitea web interface** rather than the command line:

```text
Create Pull Request
        ↓
Set source branch
story/fox-and-grapes
        ↓
Set destination branch
master
        ↓
Set PR title
Added fox-and-grapes story
        ↓
Assign tom as reviewer
        ↓
Login as tom
        ↓
Review Pull Request
        ↓
Approve Pull Request
        ↓
Merge Pull Request
```

The final result was that the `story/fox-and-grapes` branch was successfully merged into the `master` branch after review and approval.

**Day 29 completed successfully.** 
