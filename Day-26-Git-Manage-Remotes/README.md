# Day 26 - Git Remote Repository Management

## Challenge Overview

This is Day 26 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Git Remote Management, Repository Updates, Committing Changes, and Pushing Changes to a Remote Repository**.

The xFusionCorp development team had made updates to the project maintained in a Git repository. The repository was available on the Storage Server and had already been cloned to the local server.

The task was to add a new Git remote, copy a provided `index.html` file into the repository, commit the changes to the `master` branch, and push the branch to the newly configured remote.

### Challenge Requirement

The repository was located at:

```text
/usr/src/kodekloudrepos/official
```

The Git repository that needed to be added as a new remote was:

```text
/opt/xfusioncorp_official.git
```

The task requirements were:

1. Add a new remote named `dev_official`.
2. Point `dev_official` to `/opt/xfusioncorp_official.git`.
3. Copy `/tmp/index.html` into the repository.
4. Add the file to Git staging.
5. Commit the file to the `master` branch.
6. Push the `master` branch to the new `dev_official` remote.

---

## Objectives

The objectives of this challenge are:

* Access the server using SSH.
* Navigate to the existing Git repository.
* Inspect the existing Git remotes.
* Add a new Git remote.
* Understand Git remote names and URLs.
* Copy a file into a Git repository.
* Check the Git working tree.
* Stage a new file.
* Commit changes to the `master` branch.
* Push the `master` branch to a specific remote.
* Verify the remote and pushed changes.

---

## Environment

| Item              | Details                            |
| ----------------- | ---------------------------------- |
| Challenge         | 100 Days of DevOps                 |
| Platform          | KodeKloud                          |
| Day               | 26                                 |
| Repository        | `/usr/src/kodekloudrepos/official` |
| New Remote        | `dev_official`                     |
| Remote Repository | `/opt/xfusioncorp_official.git`    |
| File              | `/tmp/index.html`                  |
| Branch            | `master`                           |
| Operation         | Add, Commit, Push                  |
| Status            | Completed                          |

---

# Solution

## Step 1: Access the Server

First, I accessed the required server using SSH.

The SSH command format is:

```bash
ssh <username>@<server-hostname>
```

After connecting, I verified the server:

```bash
hostname
```

---

## Step 2: Navigate to the Repository

The existing repository was located at:

```text
/usr/src/kodekloudrepos/official
```

I moved into the repository:

```bash
cd /usr/src/kodekloudrepos/official
```

---

## Step 3: Check the Git Repository Status

Before making any changes, I checked the current Git status:

```bash
git status
```

This helped me understand the current state of the repository and make sure there were no unexpected changes.

---

## Step 4: Check Existing Git Remotes

I checked the existing Git remotes using:

```bash
git remote -v
```

This displayed the currently configured remote repositories.

---

## Step 5: Add the New Remote

The challenge required adding a new remote named:

```text
dev_official
```

and pointing it to:

```text
/opt/xfusioncorp_official.git
```

I added the remote using:

```bash
git remote add dev_official /opt/xfusioncorp_official.git
```

---

## Step 6: Verify the New Remote

After adding the remote, I verified it using:

```bash
git remote -v
```

The new remote should appear similar to:

```text
dev_official    /opt/xfusioncorp_official.git (fetch)
dev_official    /opt/xfusioncorp_official.git (push)
```

This confirmed that the new remote was configured correctly.

---

## Step 7: Check the Current Branch

Before committing the new file, I checked the current branch:

```bash
git branch
```

The required branch for this task was:

```text
master
```

If necessary, I switched to the master branch:

```bash
git checkout master
```

---

## Step 8: Copy index.html into the Repository

The required file was located at:

```text
/tmp/index.html
```

I copied it into the Git repository:

```bash
cp /tmp/index.html .
```

The `.` represents the current repository directory.

---

## Step 9: Verify the File

I checked the repository contents:

```bash
ls -l
```

I also checked the Git status:

```bash
git status
```

The newly copied file should appear as an untracked file:

```text
Untracked files:
  index.html
```

---

## Step 10: Add the File to Git

I staged the new file:

```bash
git add index.html
```

Then I verified the staging area:

```bash
git status
```

The file should now appear under:

```text
Changes to be committed
```

---

## Step 11: Commit the Changes

I committed the staged file to the `master` branch:

```bash
git commit -m "Add index.html"
```

The commit message describes the change made to the repository.

---

## Step 12: Push Master Branch to the New Remote

Finally, I pushed the `master` branch to the newly created `dev_official` remote:

```bash
git push dev_official master
```

This pushed the local `master` branch and its commit to:

```text
/opt/xfusioncorp_official.git
```

---

## Step 13: Verify the Final Repository

I checked the repository status:

```bash
git status
```

I also verified the remotes:

```bash
git remote -v
```

And checked the commit history:

```bash
git log --oneline
```

These commands helped confirm that the file was committed and the repository was configured correctly.

---

# Git Workflow Used

The complete workflow for this challenge was:

```text
Access Server
     ↓
Navigate to Repository
     ↓
Check Git Status
     ↓
Check Existing Remotes
     ↓
Add dev_official Remote
     ↓
Verify Remote
     ↓
Switch/Verify master Branch
     ↓
Copy index.html
     ↓
git add
     ↓
git commit
     ↓
git push dev_official master
     ↓
Verify Repository
```

---


# What I Learned

From this challenge, I learned and practiced:

* How Git remotes work.
* How to add a new remote repository.
* How to use a custom remote name such as `dev_official`.
* How to verify Git remotes using `git remote -v`.
* How to work with the `master` branch.
* How to copy files into an existing Git repository.
* How to stage files using `git add`.
* How to create commits using `git commit`.
* How to push a specific branch to a specific remote.
* The difference between a local repository and a remote repository.
* How important it is to verify the remote before pushing changes.

# Challenge Status

Day 26 — Completed Successfully 

**Git remote configured, `index.html` committed to master, and changes pushed successfully to the new `dev_official` remote.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
