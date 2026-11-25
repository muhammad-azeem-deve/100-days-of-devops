# Day 80 - Jenkins Chained Builds

## Challenge Overview

This is Day 80 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Jenkins Chained Builds and Automated Deployment** in the Stratos Datacenter.

The objective was to create two Jenkins jobs. The first job was responsible for deploying website files from a Gitea repository to App Server 1, while the second job was responsible for restarting the Apache (`httpd`) service.

The second job had to execute automatically, but only when the first job completed successfully with a **Stable** build status.

### Challenge Requirement

The task required creating two Jenkins jobs:

**1. Upstream Job: `xfusion-app-deployment`**

- Pull the `master` branch from the Gitea `web` repository.
- Deploy the website files to `/var/www/html` on App Server 1.
- Use an authorized sudo-capable user for deployment.

**2. Downstream Job: `manage-services`**

- Restart the Apache (`httpd`) service on App Server 1.
- Run automatically after `xfusion-app-deployment` finishes successfully.
- Trigger only when the upstream job's build status is **Stable**.

**Final Requirement:**

The website must be accessible through:

```text
http://stlb01:8091
```

The website should load directly from the root URL without requiring a `/web` subdirectory.

---

## Objectives

The objectives of this challenge are:

- Understand Jenkins chained builds.
- Create an upstream Jenkins job.
- Create a downstream Jenkins job.
- Connect Jenkins with a Gitea Git repository.
- Pull the `master` branch from the repository.
- Deploy website files to App Server 1.
- Configure SSH access for deployment.
- Use a sudo-capable user for deployment tasks.
- Troubleshoot Git dubious ownership errors.
- Configure downstream build triggers.
- Trigger the downstream job only after a stable upstream build.
- Restart Apache using Jenkins.
- Verify the complete deployment through the website URL.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 80 |
| CI/CD Tool | Jenkins |
| Git Platform | Gitea |
| Repository | `web` |
| Branch | `master` |
| Upstream Job | `xfusion-app-deployment` |
| Downstream Job | `manage-services` |
| Application Server | App Server 1 |
| Web Server | Apache (`httpd`) |
| Deployment Directory | `/var/www/html` |
| Website URL | `http://stlb01:8091` |
| Trigger Condition | Stable |
| Status | Completed |

---

# Solution

## Step 1: Access Jenkins

First, I opened the Jenkins dashboard using the provided Jenkins URL and credentials.

I checked the Jenkins dashboard to make sure that Jenkins was running properly.

---

## Step 2: Install Required Plugins

Before creating the jobs, I checked the installed Jenkins plugins.

Depending on the available environment, the required plugins included:

- Git Plugin
- Git Client Plugin
- SSH/deployment plugin, if required by the chosen deployment method.

If a required plugin was missing, I installed it through:

```text
Manage Jenkins
    ↓
Plugins
    ↓
Available Plugins
    ↓
Search Required Plugin
    ↓
Install
```

After installation, I restarted Jenkins when required.

---

## Step 3: Create the Upstream Job

I created the first Jenkins job with the following name:

```text
xfusion-app-deployment
```

I selected **Freestyle Project**.

This job was responsible for pulling the website source code from Gitea and deploying it to App Server 1.

### Configure Source Code Management

Inside the job configuration, I selected:

```text
Source Code Management → Git
```

I entered the Gitea repository URL for the `web` repository.

I configured the branch:

```text
*/master
```

This ensured that Jenkins pulled the latest code from the master branch.

### Configure Deployment

Next, I configured the deployment process to transfer the repository contents to App Server 1.

The destination directory was:

```text
/var/www/html
```

The deployment needed to use an authorized sudo-capable user on the destination server.

The important point was to deploy the contents of the repository directly into `/var/www/html`, rather than creating an additional `web` directory.

---

## Step 4: Troubleshoot Git Dubious Ownership

During the configuration, I encountered a Git error:

```text
fatal: detected dubious ownership in repository
```

This error occurred because Git considered the repository directory ownership unsafe.

To fix the issue, I configured the repository workspace as a safe directory.

The command used was:

```bash
git config --global --add safe.directory <repository-workspace-path>
```

I executed this command in the Jenkins user's environment, replacing the placeholder with the actual workspace path.

After applying this configuration, I tested the Git operation again.

---

## Step 5: Create the Downstream Job

Next, I created another Jenkins job named:

```text
manage-services
```

I selected **Freestyle Project**.

The purpose of this job was to restart the Apache service on App Server 1 after successful deployment.

### Configure Build Step

I added an Execute Shell build step with the following command:

```bash
sudo systemctl restart httpd
```

This command restarts the Apache web server.

The deployment user needed the appropriate sudo permissions to execute this command on App Server 1.

---

## Step 6: Configure the Jenkins Build Trigger

This was the most important step because the task required Jenkins chained builds.

I opened the configuration of:

```text
manage-services
```

Then I navigated to:

```text
Build Triggers
```

I enabled:

```text
Build after other projects are built
```

In the upstream project field, I entered:

```text
xfusion-app-deployment
```

I selected the following trigger condition:

```text
Stable
```

This configuration ensured that `manage-services` would execute automatically only when `xfusion-app-deployment` completed successfully with a stable build status.

Finally, I saved the configuration.

---

## Step 7: Test the Upstream Job

After configuring both jobs, I opened:

```text
xfusion-app-deployment
```

I clicked:

```text
Build Now
```

I monitored the build console output and checked whether:

- Jenkins successfully connected to Gitea.
- The master branch was checked out.
- The website files were deployed to App Server 1.
- The build completed successfully.

The upstream job needed to finish with a stable result.

---

## Step 8: Verify the Downstream Job

After the upstream job completed successfully, Jenkins automatically triggered:

```text
manage-services
```

I checked the Jenkins build history and console output to verify the chained execution.

During testing, I confirmed that:

```text
manage-services
```

was started by:

```text
xfusion-app-deployment #3
```

This confirmed that the downstream trigger was working correctly.

---

## Step 9: Verify the Website

After both Jenkins jobs completed successfully, I opened the website URL:

```text
http://stlb01:8091
```

The website loaded successfully and displayed:

```text
Welcome to KodeKloud!
```

The website was accessible directly from the root URL without using:

```text
/web
```

This confirmed that the website files were deployed to the correct directory.

---

## Troubleshooting

### Problem 1: Git Dubious Ownership Error

During the Git checkout process, Jenkins displayed:

```text
fatal: detected dubious ownership in repository
```

**Solution:**

I added the Jenkins workspace directory to Git's safe directory configuration:

```bash
git config --global --add safe.directory <repository-workspace-path>
```

This resolved the Git ownership safety issue for that workspace.

### Problem 2: Downstream Job Not Triggering

If the downstream job does not execute automatically, check:

1. Whether the upstream job completed successfully.
2. Whether the upstream project name is correct.
3. Whether the downstream build trigger is enabled.
4. Whether the trigger condition is set to Stable.
5. Whether the jobs were saved after configuration.

### Problem 3: Website Not Loading at Root URL

If the website loads only through `/web`, the repository may have been deployed into an additional subdirectory.

The correct deployment directory was:

```text
/var/www/html
```

The website files needed to be placed directly inside this directory.

---

# Jenkins Chained Build Workflow

```text
Gitea Repository
       |
       | Pull master branch
       v
xfusion-app-deployment
       |
       | Deploy website files
       v
App Server 1
/var/www/html
       |
       | Build Result: Stable
       v
manage-services
       |
       | Restart httpd
       v
Apache Web Server
       |
       v
http://stlb01:8091
       |
       v
Welcome to KodeKloud!
```

---

# What I Learned

From this challenge, I learned and practiced:

- How Jenkins Freestyle projects work.
- How to connect Jenkins with a Gitea repository.
- How to configure Git source code management.
- How to pull a specific Git branch.
- How to deploy website files to a remote Linux server.
- How to use SSH and sudo for deployment operations.
- How to troubleshoot Git dubious ownership errors.
- How to configure Jenkins upstream and downstream jobs.
- How to use the Stable build trigger condition.
- How to automate Apache service restarts.
- How Jenkins chained builds automate deployment workflows.
- How to verify deployment through a website URL.
- Why deployment directory structure matters in web hosting.

# Challenge Status

Day 80 — Completed Successfully ✅

**Both Jenkins jobs were created and connected successfully. The downstream job was triggered by the upstream stable build, Apache was restarted, and the website displayed "Welcome to KodeKloud!" at the root URL.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
