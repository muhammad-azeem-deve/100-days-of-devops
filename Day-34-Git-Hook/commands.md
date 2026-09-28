# Day 34 - Commands

This file contains the commands used during **Day 34 of my 100 Days of DevOps Challenge**.

The objective was to merge a feature branch into `master` and configure a Git `post-update` hook that automatically creates an annotated release tag whenever `master` is updated.

---

# Step 1: Verify the User

Check the current user:

```bash
whoami
```

Expected output:

```text
natasha
```

---

# Step 2: Access the Working Repository

```bash
cd /usr/src/kodekloudrepos/blog
```

Check the repository status:

```bash
git status
```

---

# Step 3: Check Branches

```bash
git branch
```

---

# Step 4: Switch to Master

```bash
git checkout master
```

Verify:

```bash
git branch
```

---

# Step 5: Merge the Feature Branch

Replace `<feature-branch>` with the actual feature branch name:

```bash
git merge <feature-branch>
```

Check the result:

```bash
git status
```

---

# Step 6: Push Master

Push the updated `master` branch:

```bash
git push origin master
```

---

# Step 7: Check the Bare Repository

The centralized bare repository is:

```text
/opt/blog.git
```

Check its contents:

```bash
ls -la /opt/blog.git
```

Check the hooks directory:

```bash
ls -la /opt/blog.git/hooks
```

---

# Step 8: Create the Post-Update Hook

Open the hook:

```bash
nano /opt/blog.git/hooks/post-update
```

Add:

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

Save and exit.

---

# Step 9: Make the Hook Executable

Only change the permission of the hook:

```bash
chmod +x /opt/blog.git/hooks/post-update
```

Verify:

```bash
ls -l /opt/blog.git/hooks/post-update
```

---

# Step 10: Configure Git Identity

Set the Git username:

```bash
git config --global user.name "natasha"
```

Set the Git email:

```bash
git config --global user.email "natasha@example.com"
```

Verify:

```bash
git config --global user.name
git config --global user.email
```

---

# Step 11: Test the Hook

Go back to the working repository:

```bash
cd /usr/src/kodekloudrepos/blog
```

Make sure you are on master:

```bash
git checkout master
```

Push master:

```bash
git push origin master
```

The push should trigger:

```text
/opt/blog.git/hooks/post-update
```

---

# Step 12: Check the Generated Tag

List the tags in the bare repository:

```bash
git --git-dir=/opt/blog.git tag
```

Expected output:

```text
release-2026-09-28
```

The date will automatically correspond to the current date.

---

# Step 13: Verify the Annotated Tag

Use:

```bash
git --git-dir=/opt/blog.git show release-$(date +%Y-%m-%d)
```

This displays the annotated tag information and the commit to which the tag points.

---

# Step 14: Verify Master Reference

Check the master branch:

```bash
git --git-dir=/opt/blog.git rev-parse refs/heads/master
```

Check the generated tag:

```bash
git --git-dir=/opt/blog.git rev-list -n 1 release-$(date +%Y-%m-%d)
```

The commit references should correspond to the updated `master` commit.

---

# Troubleshooting Commands

## Check Repository

```bash
git status
```

---

## Check Branches

```bash
git branch
```

---

## Check Remote

```bash
git remote -v
```

---

## Check Hook

```bash
ls -l /opt/blog.git/hooks/post-update
```

---

## Check Git Identity

```bash
git config --global user.name
git config --global user.email
```

---

## Check Existing Tags

```bash
git --git-dir=/opt/blog.git tag
```

---

## Check Master Reference

```bash
git --git-dir=/opt/blog.git rev-parse refs/heads/master
```

---

## Check Hook Repository Path

```bash
git --git-dir=/opt/blog.git rev-parse --is-bare-repository
```

Expected output:

```text
true
```

---

# Understanding the Hook

The first important command in the hook is:

```bash
unset GIT_DIR
```

This removes the `GIT_DIR` environment variable inherited from the Git remote operation.

The repository is then explicitly specified:

```bash
git --git-dir="/opt/blog.git"
```

This prevents Git from using an incorrect repository context.

---

# Understanding the Tag Command

The hook creates the tag using:

```bash
TAG="release-$(date +%Y-%m-%d)"
```

and:

```bash
git --git-dir="$REPO" tag -a "$TAG" "$NEWREV" -m "Release $TAG"
```

For example:

```text
release-2026-09-28
```

The `-a` option means that the tag is an **annotated tag**.

---

# Quick Command Summary

```bash
# Verify user
whoami

# Access repository
cd /usr/src/kodekloudrepos/blog

# Check status
git status

# Check branches
git branch

# Switch to master
git checkout master

# Merge feature branch
git merge <feature-branch>

# Push master
git push origin master

# Configure Git identity
git config --global user.name "natasha"
git config --global user.email "natasha@example.com"

# Create hook
nano /opt/blog.git/hooks/post-update

# Make only the hook executable
chmod +x /opt/blog.git/hooks/post-update

# Verify hook
ls -l /opt/blog.git/hooks/post-update

# Push again to trigger the hook
cd /usr/src/kodekloudrepos/blog
git push origin master

# Check generated tags
git --git-dir=/opt/blog.git tag

# Show today's annotated tag
git --git-dir=/opt/blog.git show release-$(date +%Y-%m-%d)
```

---

# Important Troubleshooting Lessons

## 1. `GIT_DIR` Problem

If Git inside the hook reports that the repository cannot be found, use:

```bash
unset GIT_DIR
```

and explicitly specify:

```bash
git --git-dir=/opt/blog.git
```

---

## 2. Incorrect Repository Path

Make sure the active repository is:

```text
/opt/blog.git
```

and not an older path such as:

```text
/opt/ecommerce.git
```

The working repository is:

```text
/usr/src/kodekloudrepos/blog
```

---

## 3. Git Identity Error

If you receive:

```text
Committer identity unknown
```

configure:

```bash
git config --global user.name "natasha"
git config --global user.email "natasha@example.com"
```

Then trigger the hook again by pushing the required update.

---

## 4. Hook Permission

If the hook does not execute, verify:

```bash
ls -l /opt/blog.git/hooks/post-update
```

Make only the hook executable:

```bash
chmod +x /opt/blog.git/hooks/post-update
```

---

# Final Verification

The most important final command is:

```bash
git --git-dir=/opt/blog.git tag
```

The expected result is a tag following:

```text
release-YYYY-MM-DD
```

For the current date used during this task:

```text
release-2026-09-28
```

The task is successfully completed when the automatically generated annotated release tag appears after updating `master`.

**Day 34 completed successfully.** 🚀
