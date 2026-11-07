# Day 69 - Install Jenkins Plugins (Git and GitLab)

## Challenge Overview

This is Day 69 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Jenkins Plugin Management** and required installing two plugins in Jenkins: the **Git Plugin** and the **GitLab Plugin**.

Jenkins uses plugins to extend its functionality and integrate with different development tools and platforms.

### Challenge Requirement

Install the following two plugins in Jenkins:

1. Git Plugin
2. GitLab Plugin

The objective was to access the Jenkins dashboard, navigate to the plugin manager, search for the required plugins, install them, and verify that both plugins were successfully installed.

---

## Objectives

The objectives of this challenge are:

- Access the Jenkins dashboard.
- Navigate to Manage Jenkins.
- Open the Jenkins Plugin Manager.
- Search for the Git Plugin.
- Search for the GitLab Plugin.
- Select both required plugins.
- Install the plugins successfully.
- Verify both plugins in the installed plugins section.
- Understand the importance of Jenkins plugins.
- Learn how Jenkins integrates with Git and GitLab.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 69 |
| Tool | Jenkins |
| Plugin 1 | Git Plugin |
| Plugin 2 | GitLab Plugin |
| Installation Method | Jenkins Dashboard |
| Verification | Installed Plugins |
| Status | Completed |

---

# Solution

## Step 1: Access Jenkins Dashboard

First, I opened the Jenkins dashboard using the browser.

The general URL format was:

```text
http://<jenkins-server-ip>:8080
```

I logged in using my Jenkins credentials.

---

## Step 2: Open Manage Jenkins

After accessing the dashboard, I navigated to:

```text
Dashboard → Manage Jenkins
```

From there, I opened the **Plugins** section to manage Jenkins plugins.

---

## Step 3: Open Available Plugins

Inside the Plugin Manager, I selected the **Available plugins** tab.

This section contains plugins that can be installed in Jenkins.

I used the search option to find the required plugins.

---

## Step 4: Select Git Plugin

First, I searched for:

```text
Git
```

I located the plugin named **Git Plugin** and selected its checkbox.

The Git Plugin allows Jenkins to interact with Git repositories and retrieve source code from version control systems.

---

## Step 5: Select GitLab Plugin

Next, I searched for:

```text
GitLab
```

I located the plugin named **GitLab Plugin** and selected its checkbox.

The GitLab Plugin helps integrate Jenkins with GitLab and supports communication between the two platforms.

---

## Step 6: Install Both Plugins

After selecting both plugins, I clicked the installation option:

```text
Install
```

or:

```text
Install without restart
```

depending on the available option.

I waited until Jenkins completed the installation process.

If Jenkins required a restart, I followed the displayed instructions.

---

## Step 7: Verify Installed Plugins

After installation, I navigated to:

```text
Manage Jenkins → Plugins → Installed plugins
```

I searched for both plugins:

```text
Git
GitLab
```

Both plugins appeared in the installed plugins list, confirming that the installation was successful.

---

# Understanding Jenkins Plugins

### 1. Git Plugin

The Git Plugin allows Jenkins to work with Git repositories.

Its main uses include:

- Cloning Git repositories.
- Checking out source code.
- Managing Git branches and commits.
- Integrating source code management with Jenkins jobs.
- Supporting automated build pipelines using Git repositories.

### 2. GitLab Plugin

The GitLab Plugin provides integration between Jenkins and GitLab.

Its main uses include:

- Connecting Jenkins with GitLab.
- Supporting GitLab-triggered build workflows.
- Integrating Jenkins jobs with GitLab events.
- Supporting automated CI/CD workflows.

---

# What I Learned

From this challenge, I learned and practiced:

- How to access the Jenkins dashboard.
- How to navigate Manage Jenkins.
- How to open the Jenkins Plugin Manager.
- How to search for Jenkins plugins.
- How to install the Git Plugin.
- How to install the GitLab Plugin.
- How Jenkins plugins extend its functionality.
- How Jenkins integrates with Git repositories.
- How Jenkins can communicate with GitLab.
- How to verify installed plugins through the Jenkins dashboard.
- The importance of plugins in building CI/CD pipelines.

# Challenge Status

Day 69 — Completed Successfully ✅

**Git and GitLab plugins installed successfully in Jenkins.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
