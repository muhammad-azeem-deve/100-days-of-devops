# Day 23 - Gitea Repository Fork

This file contains the commands and steps used during **Day 23 of my 100 Days of DevOps Challenge**.

The objective was to access the Gitea UI, log in as `jon`, find the `sarah/story-bolg` repository, fork it, and verify the fork.

---

# Step 1: Access Gitea

Open the Gitea web interface provided by the KodeKloud environment.

```text
Gitea Web UI
```

---

# Step 2: Login as jon

Log in using the provided credentials:

```text
Username: jon
Password: <provided-password>
```

After successful login, the Gitea dashboard will be displayed.

---

# Step 3: Explore Repositories

Navigate through the Gitea interface and search for:

```text
sarah/story-bolg
```

Repository owner:

```text
sarah
```

Repository name:

```text
story-bolg
```

---

# Step 4: Open the Repository

Open the repository:

```text
sarah/story-bolg
```

Review the repository information before creating the fork.

---

# Step 5: Fork the Repository

On the repository page:

1. Click **Fork**.
2. Select the `jon` account as the owner.
3. Confirm the fork operation.

The original repository:

```text
sarah/story-bolg
```

will be forked to:

```text
jon/story-bolg
```

---

# Step 6: Verify the Fork

Navigate to the repositories belonging to the `jon` account.

Verify that:

```text
jon/story-bolg
```

is present.

This confirms that the repository was successfully forked.

---

# Repository Structure

Before forking:

```text
sarah
└── story-bolg
```

After forking:

```text
sarah
└── story-bolg

jon
└── story-bolg
```

---

# Important Note

This challenge was completed through the **Gitea Web UI**, so there were no Git CLI commands required to perform the fork.

The main operation was:

```text
Gitea UI → Login as jon → Find Repository → Fork → Verify
```

---

# Quick Steps Summary

```text
1. Open Gitea UI
2. Login as jon
3. Explore repositories
4. Find sarah/story-bolg
5. Open the repository
6. Click Fork
7. Select jon as the owner
8. Confirm the fork
9. Verify jon/story-bolg
```

---

# Verification

Original repository:

```text
sarah/story-bolg
```

Forked repository:

```text
jon/story-bolg
```

The presence of `jon/story-bolg` under the `jon` account confirms that the fork operation was completed successfully.

---

# Important Lesson

A Git repository fork creates an independent copy of a repository under another user's account or namespace.

In this challenge, I learned how to use the **Gitea UI** to fork a repository rather than performing the operation through the command line.

**Day 23 completed successfully.** 
