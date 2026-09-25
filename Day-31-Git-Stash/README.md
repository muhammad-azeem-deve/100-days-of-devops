# Day 31 - Restore Stashed Git Changes

## Challenge Overview

This is Day 31 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Git Stash and Repository Management**. The Nautilus application development team had a Git repository named `beta` on the **Storage Server** in the Stratos DC.

One of the developers had previously stashed some in-progress changes in the repository. The task was to find the stashed changes, restore the stash identified as `stash@{1}`, commit the restored changes, and push the commit to the remote repository.

### Challenge Requirement

The task was to work with the following Git repository:

```text
/usr/src/kodekloudrepos/beta
```

The required steps were:

1. Access the Storage Server.
2. Navigate to the `beta` Git repository.
3. Check the available Git stashes.
4. Restore the stash with identifier `stash@{1}`.
5. Check the restored changes.
6. Commit the changes.
7. Push the commit to the `origin` remote repository.

---

## Objectives

The objectives of this challenge are:

* Access the Storage Server using SSH.
* Navigate to the required Git repository.
* Verify that the directory is a Git repository.
* Inspect available Git stashes.
* Identify `stash@{1}`.
* Restore the required stash.
* Verify the restored changes.
* Stage the changes.
* Commit the restored changes.
* Push the commit to the `origin` remote repository.
* Understand how Git stash works.
* Understand the difference between restoring and deleting a stash.

---

## Environment

| Item           | Details                        |
| -------------- | ------------------------------ |
| Challenge      | 100 Days of DevOps             |
| Platform       | KodeKloud                      |
| Day            | 31                             |
| Server         | Storage Server                 |
| Repository     | `/usr/src/kodekloudrepos/beta` |
| Git Object     | Stash                          |
| Required Stash | `stash@{1}`                    |
| Remote         | `origin`                       |
| Action         | Restore, Commit and Push       |
| Status         | Completed                      |

---

# Solution

## Step 1: Access the Storage Server

First, I accessed the **Storage Server** using SSH.

The SSH command format is:

```bash
ssh <username>@<storage-server-hostname>
```

Example:

```bash
ssh natasha@ststor01
```

---

## Step 2: Navigate to the Repository

After accessing the Storage Server, I navigated to the required Git repository:

```bash
cd /usr/src/kodekloudrepos/beta
```

---

## Step 3: Verify the Git Repository

I checked the repository status:

```bash
git status
```

This confirmed that I was working inside the correct Git repository.

I could also verify the repository information using:

```bash
git remote -v
```

This displayed the configured remote repository, including the `origin` remote.

---

## Step 4: Check Available Stashes

Next, I checked the available Git stashes:

```bash
git stash list
```

The output displayed the stashed changes.

I specifically looked for:

```text
stash@{1}
```

The stash identifier was important because the task specifically required restoring the changes stored in `stash@{1}`.

---

## Step 5: Inspect the Required Stash

Before restoring the stash, I inspected its contents:

```bash
git stash show stash@{1}
```

For more detailed information about the changes, I could use:

```bash
git stash show -p stash@{1}
```

This helped verify which changes were stored in the required stash.

---

## Step 6: Restore `stash@{1}`

I restored the required stash using:

```bash
git stash pop stash@{1}
```

This restored the changes from `stash@{1}` into the working directory.

`git stash pop` also removes the stash from the stash list after successfully applying it.

The important part of the command was:

```text
stash@{1}
```

because this was the specific stash requested in the challenge.

---

## Step 7: Check the Restored Changes

After restoring the stash, I checked the repository status:

```bash
git status
```

This showed the files that had been modified or restored from the stash.

I also checked the actual changes using:

```bash
git diff
```

This helped verify that the expected stashed changes had been restored.

---

## Step 8: Stage the Changes

After verifying the restored changes, I staged them:

```bash
git add .
```

This added the modified and restored files to the Git staging area.

I then verified the staging status:

```bash
git status
```

---

## Step 9: Commit the Changes

Next, I committed the restored changes:

```bash
git commit -m "Restore stashed changes"
```

The commit message clearly describes the purpose of the commit.

---

## Step 10: Push Changes to Origin

Finally, I pushed the commit to the remote `origin` repository:

```bash
git push origin <branch-name>
```

The branch name can be checked using:

```bash
git branch --show-current
```

For example:

```bash
git push origin master
```

or, if the repository uses another branch:

```bash
git push origin main
```

---

## Step 11: Verify the Final Repository

After pushing the changes, I verified the repository:

```bash
git status
```

I also checked the latest commit:

```bash
git log -1 --oneline
```

This confirmed that the restored changes had been committed successfully.

---


# Important Git Commands

The main commands used in this challenge were:

```bash
git stash list
```

Used to display all available stashes.

```bash
git stash show stash@{1}
```

Used to inspect the specified stash.

```bash
git stash pop stash@{1}
```

Used to restore the specified stash and remove it from the stash list after successful application.

```bash
git add .
```

Used to stage the restored changes.

```bash
git commit -m "Restore stashed changes"
```

Used to create a commit containing the restored changes.

```bash
git push origin <branch-name>
```

Used to push the commit to the remote repository.

---

# What I Learned

From this challenge, I learned and practiced:

* How Git stash stores unfinished work.
* How to list available Git stashes.
* How to identify a specific stash using `stash@{1}`.
* How to inspect the contents of a stash.
* How to restore stashed changes using `git stash pop`.
* How to check changes after restoring a stash.
* How to stage and commit restored changes.
* How to push changes to a remote repository.
* How to verify the current Git branch.
* How Git stash can be useful when temporarily saving unfinished work.

# Challenge Status

Day 31 — Completed Successfully 

**Restored the required stashed changes, committed them, and pushed the changes to the remote repository.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
