# Day 34 - Automated Git Release Tagging with Post-Update Hook

## Challenge Overview

This is Day 34 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Git Branching, Merging, Bare Repositories, and Git Hooks**.

The Nautilus application development team needed to automate a release tagging system on a centralized Git repository hosted on the Stratos DC Storage Server.

The task required merging changes from a feature branch into the `master` branch and then creating a Git `post-update` hook in the bare repository.

Whenever changes were pushed to the `master` branch, the hook had to automatically create an annotated release tag using the current date.

The required tag format was:

```text
release-YYYY-MM-DD
```

For example:

```text
release-2026-09-28
```

---

## Challenge Requirement

The main requirements were:

* Work as the `natasha` user.
* Use the local working repository:
  `/usr/src/kodekloudrepos/blog`
* Use the centralized bare repository:
  `/opt/blog.git`
* Merge the feature branch into `master`.
* Push the updated `master` branch to the bare repository.
* Create a `post-update` Git hook.
* Automatically create an annotated release tag after `master` is updated.
* Use the exact date format:
  `release-YYYY-MM-DD`
* Do not modify existing permissions except making the hook executable.
* Verify that the automated tag is created successfully.

---

## Objectives

The objectives of this challenge are:

* Understand the difference between a working Git repository and a bare repository.
* Work with Git branches.
* Merge a feature branch into `master`.
* Push changes to a centralized bare repository.
* Understand Git server-side hooks.
* Create and configure a `post-update` hook.
* Automatically generate date-based release tags.
* Create annotated Git tags.
* Understand the `GIT_DIR` environment variable.
* Use `--git-dir` to explicitly identify the bare repository.
* Configure Git user identity for automated annotated tags.
* Verify that the hook works correctly.

---

## Environment

| Item               | Details                        |
| ------------------ | ------------------------------ |
| Challenge          | 100 Days of DevOps             |
| Platform           | KodeKloud                      |
| Day                | 34                             |
| User               | `natasha`                      |
| Working Repository | `/usr/src/kodekloudrepos/blog` |
| Bare Repository    | `/opt/blog.git`                |
| Main Branch        | `master`                       |
| Hook               | `post-update`                  |
| Tag Type           | Annotated                      |
| Tag Format         | `release-YYYY-MM-DD`           |
| Status             | Completed                      |

---

# Solution

## Step 1: Switch to the natasha User

The task required working as the `natasha` user.

I verified the current user using:

```bash
whoami
```

Expected output:

```text
natasha
```

---

## Step 2: Access the Working Repository

I moved into the local working repository:

```bash
cd /usr/src/kodekloudrepos/blog
```

I verified the repository using:

```bash
git status
```

---

## Step 3: Check the Available Branches

I checked the available branches:

```bash
git branch
```

The repository contained the feature branch and the `master` branch.

---

## Step 4: Switch to Master

I switched to the `master` branch:

```bash
git checkout master
```

I then verified the current branch:

```bash
git branch
```

The `master` branch should be marked with `*`.

---

## Step 5: Merge the Feature Branch

I merged the required feature branch into `master`:

```bash
git merge <feature-branch>
```

I then checked the repository status:

```bash
git status
```

If the merge was successful, the changes from the feature branch were now part of `master`.

---

## Step 6: Push Master to the Central Repository

I pushed the updated `master` branch to the centralized bare repository:

```bash
git push origin master
```

This updated the remote `master` branch in:

```text
/opt/blog.git
```

---

# Creating the Git Post-Update Hook

## Step 7: Understand the Hook Location

The centralized repository was a bare Git repository:

```text
/opt/blog.git
```

Git server-side hooks are stored inside:

```text
/opt/blog.git/hooks/
```

The required hook was:

```text
/opt/blog.git/hooks/post-update
```

---

## Step 8: Create the Post-Update Hook

I created the hook:

```bash
nano /opt/blog.git/hooks/post-update
```

The hook script was:

```bash
#!/bin/bash

unset GIT_DIR

REPO="/opt/blog.git"

for refname in "$@"; do
    if [ "$refname" = "refs/heads/master" ]; then
        NEWREV=$(git --git-dir="$REPO" rev-parse "$refname")
        TAG="release-$(date +%Y-%m-%d)"

        git --git-dir="$REPO" tag -a "$TAG" "$NEWREV" -m "Release $TAG"
    fi
done
```

---

## Step 9: Make the Hook Executable

Only the hook script needed its executable permission changed.

I used:

```bash
chmod +x /opt/blog.git/hooks/post-update
```

I did not modify permissions on the rest of the repository.

---

## Step 10: Configure Git Identity

When creating an annotated tag, Git requires an identity.

I configured the Git identity for `natasha`:

```bash
git config --global user.name "natasha"
git config --global user.email "natasha@example.com"
```

The email can be replaced with the identity required by the lab environment.

I verified the configuration:

```bash
git config --global user.name
git config --global user.email
```

---

# Understanding the Hook

The important part of the hook is:

```bash
unset GIT_DIR
```

This removes the Git environment variable injected during the remote update operation.

The hook then explicitly identifies the bare repository:

```bash
git --git-dir="/opt/blog.git"
```

This ensures that Git commands operate on the correct repository.

---

## Automatic Release Tag

The hook creates the tag using:

```bash
TAG="release-$(date +%Y-%m-%d)"
```

The command:

```bash
date +%Y-%m-%d
```

returns the current date.

For example:

```text
2026-09-28
```

Therefore the generated tag becomes:

```text
release-2026-09-28
```

---

## Annotated Tag

The hook uses:

```bash
git --git-dir="$REPO" tag -a "$TAG" "$NEWREV" -m "Release $TAG"
```

The `-a` option creates an **annotated tag**.

The tag points to the new `master` commit.

---

# Troubleshooting

During the task, I faced several important issues.

## Issue 1 - Git Environment Isolation

Initially, the Git tag command inside the hook did not work correctly.

The problem was caused by the `GIT_DIR` environment variable being present during the remote push operation.

### Resolution

I added:

```bash
unset GIT_DIR
```

and explicitly specified the repository:

```bash
git --git-dir="/opt/blog.git"
```

This ensured that the hook operated on the correct bare repository.

---

## Issue 2 - Repository Path Changed

The lab environment used different repository names in different contexts.

An earlier environment used:

```text
/opt/ecommerce.git
```

while the active challenge used:

```text
/opt/blog.git
```

Using the old repository path caused an error similar to:

```text
fatal: not a git repository: '/opt/ecommerce.git'
```

### Resolution

I recreated the hook using the active repository:

```text
/opt/blog.git
```

and the working directory:

```text
/usr/src/kodekloudrepos/blog
```

---

## Issue 3 - Git Committer Identity

When the hook attempted to create the annotated tag, Git returned an identity error:

```text
Committer identity unknown
```

The problem occurred because the `natasha` user did not have a Git username and email configured.

### Resolution

I configured:

```bash
git config --global user.name "natasha"
git config --global user.email "natasha@example.com"
```

After configuring the Git identity, the annotated tag could be created successfully.

---

# Verification

## Check the Remote Tags

After pushing the `master` branch, I checked the tags in the bare repository:

```bash
git --git-dir=/opt/blog.git tag
```

Expected output:

```text
release-2026-09-28
```

The date will change according to the current date when the task is executed.

---

## Verify the Tag Details

I inspected the generated annotated tag:

```bash
git --git-dir=/opt/blog.git show release-$(date +%Y-%m-%d)
```

This verified that the tag existed and contained annotation information.

---

## Verify the Hook

I checked that the hook existed:

```bash
ls -l /opt/blog.git/hooks/post-update
```

The hook should have executable permissions.

---

# Complete Workflow

The complete process can be summarized as:

```text
Feature Branch
      |
      v
Working Repository
/usr/src/kodekloudrepos/blog
      |
      v
Checkout master
      |
      v
Merge Feature Branch
      |
      v
Push master
      |
      v
Bare Repository
/opt/blog.git
      |
      v
post-update Hook
      |
      v
Check master update
      |
      v
Generate current date
      |
      v
release-YYYY-MM-DD
      |
      v
Annotated Git Tag
```

---

# What I Learned

From this challenge, I learned and practiced:

* How to work with Git branches.
* How to merge feature branches into `master`.
* How bare Git repositories work.
* How a centralized Git repository can receive pushes.
* How server-side Git hooks work.
* How the `post-update` hook is triggered after a remote update.
* How to automatically create release tags.
* How to create annotated Git tags.
* How to use `date` inside shell scripts.
* Why `GIT_DIR` can cause problems inside Git hooks.
* How `unset GIT_DIR` can solve the repository context problem.
* How `--git-dir` explicitly identifies a bare repository.
* Why Git requires a user identity for annotated tags.
* How to troubleshoot repository path and Git identity errors.

# Challenge Status

Day 34 — Completed Successfully 

**Automated release tagging configured successfully using a Git `post-update` hook.**

The final workflow automatically creates:

```text
release-YYYY-MM-DD
```

whenever the `master` branch is updated.

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
