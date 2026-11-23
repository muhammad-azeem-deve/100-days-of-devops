# Day 79 - Jenkins Deployment Job

## Challenge Overview

This is Day 79 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Jenkins CI/CD Automation and Deployment**. The objective was to automate the deployment of the Nautilus team's web application using Jenkins.

Whenever a developer pushed changes to the `master` branch of the `web` repository in Gitea, Jenkins had to automatically detect the changes, build the project, and deploy the complete repository content to the Apache web server on Application Server 1.

### Challenge Requirement

Create a Jenkins job named:

```text
xfusion-app-deployment
```

The job must automatically deploy the complete content of the `web` repository to:

```text
/var/www/html
```

on Application Server 1 whenever changes are pushed to the `master` branch.

The following requirements had to be fulfilled:

- `/var/www/html` must be owned by `sarah`.
- Apache (`httpd`) must be running.
- Jenkins must use App Server 1 as its build agent.
- The Jenkins job must monitor the `master` branch.
- The deployment must happen automatically.
- The website must load through the main URL without a `/web` subdirectory.
- The `index.html` file must display:

```text
Welcome to the xFusionCorp Industries
```

---

## Objectives

The objectives of this challenge are:

- Prepare Application Server 1 for deployment.
- Start and verify the Apache web server.
- Set the ownership of `/var/www/html` to `sarah`.
- Verify Java and Git availability.
- Install the required Jenkins plugins.
- Configure SSH credentials for the `sarah` user.
- Add App Server 1 as a permanent Jenkins SSH agent.
- Create a Jenkins Freestyle project.
- Configure Gitea as the source code repository.
- Configure the `master` branch.
- Enable Poll SCM to detect changes automatically.
- Create a deployment script using `rsync`.
- Troubleshoot sudo permission issues.
- Push changes to Gitea and trigger an automatic deployment.
- Verify the final website deployment.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 79 |
| CI/CD Tool | Jenkins |
| Git Platform | Gitea |
| Repository | `web` |
| Branch | `master` |
| Jenkins Job | `xfusion-app-deployment` |
| Build Type | Freestyle Project |
| Build Agent | App Server 1 |
| Agent Label | `stapp01` |
| Deployment User | `sarah` |
| Deployment Directory | `/var/www/html` |
| Web Server | Apache (`httpd`) |
| Deployment Tool | `rsync` |
| Trigger | Poll SCM |
| Polling Schedule | Every minute |
| Status | Completed |

---

# Solution

## Step 1: Prepare Application Server 1

First, I accessed Application Server 1 using SSH.

```bash
ssh <username>@stapp01
```

I verified the server:

```bash
hostname
```

Then I started the Apache web server:

```bash
sudo systemctl start httpd
```

I checked its status:

```bash
sudo systemctl status httpd
```

Next, I changed the ownership of the website directory to `sarah`:

```bash
sudo chown -R sarah:sarah /var/www/html
```

I also verified that Java and Git were available:

```bash
java -version
git --version
```

These tools were required for Jenkins agent operations and Git repository management.

---

## Step 2: Install Required Jenkins Plugins

I opened the Jenkins dashboard and navigated to:

```text
Manage Jenkins → Plugins → Available Plugins
```

I installed the following plugins:

1. Git Plugin
2. SSH Build Agents Plugin

The Git plugin allows Jenkins to retrieve source code from Gitea.

The SSH Build Agents plugin allows Jenkins to connect to App Server 1 and execute deployment jobs remotely.

---

## Step 3: Configure Sarah's SSH Credentials

I navigated to:

```text
Manage Jenkins → Credentials → System → Global credentials
```

I selected **Add Credentials** and configured Sarah's SSH credentials.

The credentials included:

- Username: `sarah`
- Password: Sarah's SSH password
- ID: `sarah-ssh`

These credentials were used to establish an SSH connection between Jenkins and App Server 1.

---

## Step 4: Configure App Server 1 as a Jenkins Agent

I navigated to:

```text
Manage Jenkins → Nodes → New Node
```

I created a permanent agent with the following configuration:

| Setting | Value |
|---|---|
| Node Name | `stapp01` |
| Type | Permanent Agent |
| Remote Root Directory | `/home/sarah/jenkins` |
| Labels | `stapp01` |
| Launch Method | Launch agents via SSH |
| Host | App Server 1 hostname |
| Credentials | Sarah's SSH credentials |

After saving the configuration, I verified that the agent connected successfully.

This allowed Jenkins to execute deployment commands directly on App Server 1.

---

## Step 5: Create a Jenkins Freestyle Job

I returned to the Jenkins dashboard and selected:

```text
New Item
```

I entered the job name:

```text
xfusion-app-deployment
```

Then I selected:

```text
Freestyle project
```

and clicked OK.

---

## Step 6: Configure the Build Agent

Inside the job configuration, I enabled:

```text
Restrict where this project can be run
```

I entered the agent label:

```text
stapp01
```

This ensured that the deployment job would execute on Application Server 1.

---

## Step 7: Configure Git Repository

Under **Source Code Management**, I selected Git.

I entered the Gitea repository URL:

```text
http://<gitea-host>/<repository-owner>/web.git
```

I configured the branch specifier:

```text
*/master
```

This instructed Jenkins to retrieve source code from the `master` branch.

If the repository required authentication, I selected the appropriate Gitea credentials.

---

## Step 8: Configure Poll SCM

Under **Build Triggers**, I enabled:

```text
Poll SCM
```

I configured the polling schedule:

```text
* * * * *
```

This means Jenkins checks the repository every minute.

Whenever Jenkins detects new commits on the configured branch, it automatically starts the deployment job.

---

## Step 9: Configure the Deployment Script

Under **Build Steps**, I selected:

```text
Execute shell
```

The deployment script performed the following operations:

1. Corrected the ownership of `/var/www/html`.
2. Started Apache.
3. Synchronized the repository content with the web directory.
4. Excluded Git metadata from deployment.

The main deployment command was:

```bash
rsync -rv --delete --exclude=.git ./ /var/www/html/
```

The `rsync` command copied the complete repository content into the Apache document root.

### Understanding rsync options

| Option | Description |
|---|---|
| `-r` | Copies directories recursively. |
| `-v` | Displays copied files. |
| `--delete` | Removes destination files not present in the source. |
| `--exclude=.git` | Excludes the Git metadata directory. |
| `./` | Current repository directory. |
| `/var/www/html/` | Destination directory. |

---

## Step 10: Troubleshooting Sudo Permission Error

During the deployment process, I faced a sudo permission issue.

The Jenkins agent was executing commands as `sarah`, but changing directory ownership and managing Apache required elevated privileges.

Initially, the build failed because sudo required a password.

I resolved this issue by using:

```bash
sudo -S
```

The `-S` option allows sudo to read the password from standard input.

For a secure implementation, I used the concept of supplying the sudo password through a Jenkins secret credential rather than hardcoding it into the script.

Example deployment logic:

```bash
set -e

printf '%s\n' "$SUDO_PASS" | sudo -S -p '' bash -c \
'chown -R sarah:sarah /var/www/html && systemctl start httpd'

rsync -rv --delete --exclude=.git ./ /var/www/html/
```

Here, `SUDO_PASS` represents a securely injected Jenkins credential.

---

## Step 11: Troubleshooting the Extra SSH Command

I also encountered another issue in the deployment script.

Initially, I included an additional SSH command inside the build script.

However, the Jenkins job was already executing on App Server 1 through the configured SSH agent.

Therefore, an additional SSH connection was unnecessary.

I removed the extra SSH command and allowed Jenkins to execute the deployment commands directly on the agent.

This resolved the issue.

---

## Step 12: Update index.html

After completing the Jenkins configuration, I updated the website's `index.html` file in the Gitea repository.

I changed its content to:

```html
<h1>Welcome to the xFusionCorp Industries</h1>
```

This was the required website message.

---

## Step 13: Commit and Push Changes

After updating `index.html`, I committed and pushed the changes to the `master` branch.

```bash
git add index.html
git commit -m "Update website welcome message"
git push origin master
```

Because Poll SCM was enabled, Jenkins detected the new commit and automatically triggered the deployment job.

---

## Step 14: Verify Jenkins Build

I opened the Jenkins dashboard and navigated to:

```text
xfusion-app-deployment
```

I checked the Build History and opened the latest build.

Under Console Output, I verified that the deployment commands executed successfully.

The build completed successfully.

---

## Step 15: Verify Deployment on App Server 1

I connected to Application Server 1:

```bash
ssh <username>@stapp01
```

I checked the website directory:

```bash
ls -la /var/www/html
```

I verified the directory ownership:

```bash
ls -ld /var/www/html
```

I checked the Apache service:

```bash
sudo systemctl status httpd
```

I also verified the deployed website content:

```bash
cat /var/www/html/index.html
```

Expected content:

```text
Welcome to the xFusionCorp Industries
```

Finally, I opened the main website URL in the browser.

The website loaded successfully without requiring a `/web` subdirectory.

---

# Troubleshooting

During this challenge, I encountered two important problems.

### Problem 1: Sudo Required a Password

The Jenkins job initially failed because the `sarah` user needed a password to execute privileged commands.

**Solution:**

I used `sudo -S` to supply the password through standard input.

For better security, the password should be stored and injected through Jenkins credentials instead of being written directly in the build script.

### Problem 2: Extra SSH Command in Build Script

I initially included an SSH command inside the Execute Shell script.

However, the Jenkins job was already running on App Server 1 through the SSH agent.

**Solution:**

I removed the unnecessary SSH command and executed the deployment commands directly on the agent.

---

# What I Learned

From this challenge, I learned and practiced:

- How Jenkins automates application deployment.
- How Gitea and Jenkins work together.
- How to install and configure Jenkins plugins.
- How to add SSH credentials in Jenkins.
- How to configure a permanent Jenkins SSH agent.
- How to create and configure a Freestyle project.
- How to restrict Jenkins jobs to a specific build agent.
- How to configure Git as the source code repository.
- How to use Poll SCM for automatic change detection.
- How to deploy applications using `rsync`.
- How to synchronize repository content with an Apache document root.
- How to troubleshoot sudo permission errors.
- How to execute deployment commands remotely through Jenkins agents.
- How to verify automated deployment using Jenkins Console Output.
- How Continuous Deployment reduces manual deployment work.

# Challenge Status

Day 79 — Completed Successfully ✅

**Jenkins deployment automation configured successfully. Every new push to the master branch triggers an automatic deployment to App Server 1.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
