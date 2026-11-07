# Day 69 - Commands

This file contains the steps used during **Day 69 of my 100 Days of DevOps Challenge**.

The objective was to install the **Git Plugin** and **GitLab Plugin** in Jenkins and verify their successful installation.

**Note:** This challenge was completed through the Jenkins web dashboard, so the following are GUI navigation steps rather than Linux terminal commands.

---

## Step 1: Access Jenkins Dashboard

Open Jenkins in your browser:

```text
http://<jenkins-server-ip>:8080
```

Log in using Jenkins credentials.

---

## Step 2: Open Manage Jenkins

Navigate to:

```text
Dashboard → Manage Jenkins
```

---

## Step 3: Open Plugin Manager

Navigate to:

```text
Manage Jenkins → Plugins
```

---

## Step 4: Search for Git Plugin

1. Open the Available plugins section.
2. Search for:

```text
Git
```

3. Find **Git Plugin**.
4. Select its checkbox.

---

## Step 5: Search for GitLab Plugin

1. Search for:

```text
GitLab
```

2. Find **GitLab Plugin**.
3. Select its checkbox.

---

## Step 6: Install Plugins

After selecting both plugins, click:

```text
Install
```

or:

```text
Install without restart
```

Wait for the installation process to finish.

---

## Step 7: Verify Installed Plugins

Navigate to:

```text
Manage Jenkins → Plugins → Installed plugins
```

Search for:

```text
Git
GitLab
```

Verify that both plugins are listed.

---

# Quick Solution Summary

```text
1. Open Jenkins Dashboard.

2. Navigate to:
   Manage Jenkins → Plugins

3. Open:
   Available plugins

4. Search:
   Git

5. Select:
   Git Plugin

6. Search:
   GitLab

7. Select:
   GitLab Plugin

8. Click:
   Install / Install without restart

9. Wait for installation to complete.

10. Navigate to:
    Manage Jenkins → Plugins → Installed plugins

11. Verify:
    Git Plugin
    GitLab Plugin
```

---

# Plugin Information

| Plugin | Purpose |
|--------|---------|
| Git Plugin | Allows Jenkins to interact with Git repositories. |
| GitLab Plugin | Integrates Jenkins with GitLab. |

---

# Final Verification Checklist

- [ ] Jenkins dashboard accessed successfully.
- [ ] Manage Jenkins opened.
- [ ] Plugin Manager opened.
- [ ] Git Plugin selected and installed.
- [ ] GitLab Plugin selected and installed.
- [ ] Both plugins verified in Installed plugins.

---

# Important Lesson

Jenkins plugins extend the functionality of Jenkins and allow it to integrate with external tools.

For this challenge, the two important plugins were:

```text
Git Plugin
GitLab Plugin
```

The final verification was performed through:

```text
Manage Jenkins → Plugins → Installed plugins
```

Both plugins must appear in the installed plugins list to complete the task successfully.

**Day 69 completed successfully.** 🚀
