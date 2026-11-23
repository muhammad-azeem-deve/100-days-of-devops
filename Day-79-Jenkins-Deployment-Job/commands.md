# Day 79 - Commands

This file contains the commands used during **Day 79 of my 100 Days of DevOps Challenge**.

The objective was to configure Jenkins to automatically deploy the Gitea `web` repository to `/var/www/html` on App Server 1 whenever changes were pushed to the `master` branch.

---

# Step 1: Access App Server 1

Connect to Application Server 1:

```bash
ssh <username>@stapp01
```

Verify the server:

```bash
hostname
```

---

# Step 2: Start Apache

Start the Apache web server:

```bash
sudo systemctl start httpd
```

Check the service status:

```bash
sudo systemctl status httpd
```

---

# Step 3: Change Website Directory Ownership

Set Sarah as the owner of the website directory:

```bash
sudo chown -R sarah:sarah /var/www/html
```

Verify ownership:

```bash
ls -ld /var/www/html
```

---

# Step 4: Verify Java and Git

Check Java:

```bash
java -version
```

Check Git:

```bash
git --version
```

These tools are required for Jenkins agent and Git operations.

---

# Step 5: Jenkins Plugin Configuration

Install the following plugins through the Jenkins dashboard:

```text
Git Plugin
SSH Build Agents Plugin
```

Navigation:

```text
Manage Jenkins → Plugins → Available Plugins
```

---

# Step 6: Configure SSH Credentials

Add Sarah's SSH credentials through:

```text
Manage Jenkins → Credentials → System → Global credentials
```

Credential details:

```text
Username: sarah
Password: <sarah-password>
ID: sarah-ssh
```

---

# Step 7: Configure Jenkins Agent

Create a permanent SSH agent.

Configuration:

```text
Node Name: stapp01
Remote Root Directory: /home/sarah/jenkins
Labels: stapp01
Launch Method: Launch agents via SSH
Host: <app-server-1-hostname>
Credentials: sarah-ssh
```

Verify that the agent is online in Jenkins.

---

# Step 8: Create Jenkins Job

Create a Freestyle project:

```text
xfusion-app-deployment
```

Restrict the job to:

```text
stapp01
```

---

# Step 9: Configure Git Repository

Under Source Code Management → Git:

```text
Repository URL:
http://<gitea-host>/<repository-owner>/web.git
```

Branch Specifier:

```text
*/master
```

---

# Step 10: Configure Poll SCM

Under Build Triggers, enable Poll SCM.

Schedule:

```text
* * * * *
```

This checks the Git repository every minute.

---

# Step 11: Deployment Script

Under Build Steps, select Execute shell.

The main deployment command is:

```bash
rsync -rv --delete --exclude=.git ./ /var/www/html/
```

This synchronizes the complete repository content with the Apache website directory.

---

# Step 12: Sudo Permission Handling

The Jenkins agent runs as `sarah`, but some operations require root privileges.

The `-S` option allows sudo to read the password from standard input.

Example:

```bash
printf '%s\n' "$SUDO_PASS" | sudo -S -p '' bash -c \
'chown -R sarah:sarah /var/www/html && systemctl start httpd'
```

Here, `SUDO_PASS` should be supplied securely through a Jenkins secret credential.

Do not hardcode the actual password in the build script.

---

# Step 13: Complete Deployment Script

The deployment logic used in the Execute Shell build step:

```bash
set -e

# Fix website directory ownership and start Apache
printf '%s\n' "$SUDO_PASS" | sudo -S -p '' bash -c \
'chown -R sarah:sarah /var/www/html && systemctl start httpd'

# Deploy repository content to Apache document root
rsync -rv --delete --exclude=.git ./ /var/www/html/
```

This script performs the following operations:

1. Stops execution if a command fails.
2. Changes ownership of `/var/www/html`.
3. Starts Apache.
4. Copies repository files to the website directory.
5. Excludes `.git` from deployment.

---

# Step 14: Update index.html

Open the repository's `index.html` file and update the content:

```html
<h1>Welcome to the xFusionCorp Industries</h1>
```

---

# Step 15: Commit and Push Changes

Add the updated file:

```bash
git add index.html
```

Commit the changes:

```bash
git commit -m "Update website welcome message"
```

Push changes to the master branch:

```bash
git push origin master
```

Jenkins detects the new commit through Poll SCM and automatically triggers the deployment job.

---

# Step 16: Verify Jenkins Build

Open Jenkins and navigate to:

```text
xfusion-app-deployment
```

Check:

```text
Build History → Latest Build → Console Output
```

Verify that the build completes successfully.

---

# Step 17: Verify Deployment on App Server 1

Connect to App Server 1:

```bash
ssh <username>@stapp01
```

Check deployed files:

```bash
ls -la /var/www/html
```

Check directory ownership:

```bash
ls -ld /var/www/html
```

Check Apache status:

```bash
sudo systemctl status httpd
```

Check website content:

```bash
cat /var/www/html/index.html
```

Expected content:

```text
Welcome to the xFusionCorp Industries
```

---

# Important Jenkins Configuration Summary

| Configuration | Value |
|---|---|
| Job Name | `xfusion-app-deployment` |
| Job Type | Freestyle Project |
| Agent | `stapp01` |
| Repository | `web` |
| Branch | `*/master` |
| Trigger | Poll SCM |
| Schedule | `* * * * *` |
| Deployment User | `sarah` |
| Destination | `/var/www/html` |
| Web Server | `httpd` |

---

# Important Commands Summary

```bash
# Connect to App Server 1
ssh <username>@stapp01

# Verify server
hostname

# Start Apache
sudo systemctl start httpd

# Check Apache
sudo systemctl status httpd

# Change website directory ownership
sudo chown -R sarah:sarah /var/www/html

# Verify directory ownership
ls -ld /var/www/html

# Verify Java
java -version

# Verify Git
git --version

# Deploy website files
rsync -rv --delete --exclude=.git ./ /var/www/html/

# Check deployed files
ls -la /var/www/html

# Verify website content
cat /var/www/html/index.html
```

---

# Important Lesson

This challenge taught me how to automate the complete deployment process using Jenkins.

The most important components were:

```text
Gitea → Jenkins → SSH Agent → App Server 1 → Apache
```

The most important deployment command was:

```bash
rsync -rv --delete --exclude=.git ./ /var/www/html/
```

The most important Jenkins configurations were:

```text
Job Name: xfusion-app-deployment
Agent Label: stapp01
Branch: */master
Poll SCM: * * * * *
```

With this configuration, every new commit pushed to the `master` branch can trigger an automatic deployment.

**Day 79 completed successfully.** 🚀
