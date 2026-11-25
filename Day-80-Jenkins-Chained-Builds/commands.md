# Day 80 - Commands

This file contains the commands used during **Day 80 of my 100 Days of DevOps Challenge**.

The objective was to create two Jenkins jobs, deploy website files from Gitea to App Server 1, and automatically restart Apache after a successful deployment.

---

# Step 1: Verify Jenkins User

Check the current Linux user:

```bash
whoami
```

Check Jenkins workspace ownership:

```bash
ls -ld <repository-workspace-path>
```

---

# Step 2: Fix Git Dubious Ownership Error

If Jenkins displays:

```text
fatal: detected dubious ownership in repository
```

Add the repository workspace to Git's safe directory configuration.

```bash
git config --global --add safe.directory <repository-workspace-path>
```

Verify the safe directory configuration:

```bash
git config --global --get-all safe.directory
```

Check Git configuration:

```bash
git config --global --list
```

**Important:** Run these Git configuration commands as the Jenkins user, not as an unrelated user.

---

# Step 3: Verify Git Repository

Check the repository status:

```bash
git status
```

Check the current branch:

```bash
git branch
```

Check recent commits:

```bash
git log --oneline -5
```

The required branch for this challenge was:

```text
master
```

---

# Step 4: Connect to App Server 1

Connect to App Server 1 using the authorized deployment user:

```bash
ssh <sudo-user>@stlb01
```

Verify the server:

```bash
hostname
```

Check the deployment directory:

```bash
ls -la /var/www/html
```

---

# Step 5: Verify Deployment Directory Permissions

Check the ownership and permissions of the website directory:

```bash
ls -ld /var/www/html
```

The deployment user must have the necessary permissions to deploy website files into this directory.

---

# Step 6: Deploy Website Files

One possible deployment method is using `rsync` over SSH.

From the Jenkins workspace, transfer the repository contents to a temporary directory on App Server 1:

```bash
rsync -av --delete ./ <sudo-user>@stlb01:/tmp/web-deploy/
```

Then connect to App Server 1 and copy the files into the web root:

```bash
sudo rsync -av --delete /tmp/web-deploy/ /var/www/html/
```

This example assumes `rsync` is installed, SSH authentication is configured, and the deployment user has the required sudo permissions.

**Important:** The trailing slash in `/tmp/web-deploy/` ensures that the contents are copied directly into `/var/www/html/`, rather than creating an additional directory.

The actual deployment method configured in Jenkins may differ.

---

# Step 7: Verify Website Files

Check the deployed files:

```bash
ls -la /var/www/html
```

Verify that the website's main page exists:

```bash
ls -l /var/www/html/index.html
```

---

# Step 8: Restart Apache

The downstream Jenkins job `manage-services` executes the following command on App Server 1:

```bash
sudo systemctl restart httpd
```

This restarts the Apache web server.

---

# Step 9: Verify Apache Status

Check the Apache service status:

```bash
sudo systemctl status httpd
```

The expected service status is:

```text
active (running)
```

---

# Step 10: Verify Apache Listening Port

Check whether Apache is listening on port 8091:

```bash
sudo ss -tlnp | grep 8091
```

---

# Step 11: Test Website Using curl

From a machine that can access the application server:

```bash
curl http://stlb01:8091
```

Expected website content:

```text
Welcome to KodeKloud!
```

---

# Jenkins Job Configuration

## Job 1: xfusion-app-deployment

Job type:

```text
Freestyle Project
```

Source Code Management:

```text
Git
```

Repository:

```text
Gitea web repository URL
```

Branch:

```text
*/master
```

Deployment destination:

```text
/var/www/html
```

Purpose:

```text
Pull source code from Gitea and deploy website files to App Server 1.
```

---

## Job 2: manage-services

Job type:

```text
Freestyle Project
```

Build step:

```text
Execute Shell
```

Command:

```bash
sudo systemctl restart httpd
```

Purpose:

```text
Restart Apache on App Server 1.
```

---

# Jenkins Build Trigger Configuration

Open the `manage-services` job.

Navigate to:

```text
Configure
    ↓
Build Triggers
    ↓
Build after other projects are built
```

Upstream project:

```text
xfusion-app-deployment
```

Trigger condition:

```text
Stable
```

This ensures that the downstream job executes only after the upstream job completes successfully with a stable result.

---

# Testing the Jenkins Chain

Run the upstream job:

```text
xfusion-app-deployment
```

Check the upstream build result:

```text
Stable
```

After successful completion, Jenkins should automatically trigger:

```text
manage-services
```

During my testing, I verified that the downstream job was started by:

```text
xfusion-app-deployment #3
```

This confirmed that the Jenkins chained build was working correctly.

---

# Quick Command Summary

```bash
# Check current user
whoami

# Check workspace ownership
ls -ld <repository-workspace-path>

# Fix Git dubious ownership
git config --global --add safe.directory <repository-workspace-path>

# Verify Git safe directories
git config --global --get-all safe.directory

# Check Git repository
git status

# Check current branch
git branch

# Connect to App Server 1
ssh <sudo-user>@stlb01

# Verify server
hostname

# Check website directory
ls -la /var/www/html

# Check website directory ownership
ls -ld /var/www/html

# Restart Apache
sudo systemctl restart httpd

# Verify Apache status
sudo systemctl status httpd

# Check Apache listening port
sudo ss -tlnp | grep 8091

# Test website
curl http://stlb01:8091
```

---

# Important Lessons

The most important concepts from this challenge were:

**1. Upstream Job**

`xfusion-app-deployment` pulls the master branch from Gitea and deploys website files.

**2. Downstream Job**

`manage-services` restarts Apache after a successful deployment.

**3. Stable Build Trigger**

The downstream job must execute only when the upstream job finishes with a Stable result.

**4. Git Safe Directory**

Git's `safe.directory` configuration helps resolve dubious ownership errors.

**5. Correct Deployment Directory**

The website files must be deployed directly into:

```text
/var/www/html
```

**6. Final Verification**

The website must be accessible at:

```text
http://stlb01:8091
```

Expected content:

```text
Welcome to KodeKloud!
```

---

**Day 80 completed successfully!** 🚀
