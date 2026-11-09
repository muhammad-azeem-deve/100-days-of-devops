# Day 71 - Commands

This file contains the commands used during **Day 71 of my 100 Days of DevOps Challenge**.

The objective was to configure a Jenkins job that installs a package specified through the `PACKAGE` parameter on the Storage Server.

---

# Step 1: Access Jenkins

Open the Jenkins dashboard through the KodeKloud lab interface.

Log in using the administrator credentials provided by the lab.

---

# Step 2: Install SSH Plugin

Navigate to:

```text
Manage Jenkins → Plugins → Available Plugins
```

Search for:

```text
SSH
```

Install the required SSH plugin.

Restart Jenkins when the installation is complete if required.

---

# Step 3: Add Storage Server Credentials

Navigate to:

```text
Manage Jenkins
    → Credentials
    → System
    → Global Credentials
    → Add Credentials
```

Configure:

```text
Kind: Username with password
Username: <storage-server-username>
Password: <storage-server-password>
ID: storage
Description: Storage Server SSH Credentials
```

Save the credentials.

---

# Step 4: Configure SSH Remote Host

Navigate to:

```text
Manage Jenkins → System
```

Find the SSH Remote Hosts section.

Add:

```text
Hostname: <storage-server-hostname>
Port: 22
Credentials: storage
```

Click:

```text
Check Connection
```

Save the configuration after successful connection verification.

---

# Step 5: Create Jenkins Node (If Required)

Navigate to:

```text
Manage Jenkins → Nodes → New Node
```

Enter:

```text
Node Name: Storage_Server
Type: Permanent Agent
Remote Root Directory: /home/<username>/jenkins
Labels: storage
Launch Method: Launch agents via SSH
```

Configure the SSH connection and credentials if using the agent-node approach.

Save and verify the node status.

**Note:** This is separate from the SSH Remote Host configuration used by the standard Day 71 remote build step.

---

# Step 6: Create Jenkins Job

From the Jenkins Dashboard:

```text
New Item
```

Enter:

```text
Job Name: install-packages
```

Select:

```text
Freestyle Project
```

Click OK.

---

# Step 7: Add String Parameter

Inside the job configuration, enable:

```text
This project is parameterized
```

Add a String Parameter:

```text
Name: PACKAGE
Default Value: vim-enhanced
Description: Package name to install on Storage Server
```

Save the parameter configuration.

---

# Step 8: Configure Build Step

Navigate to:

```text
Build Steps
```

Select:

```text
Execute shell script on remote host using SSH
```

Choose the configured Storage Server SSH connection.

Add the remote installation command:

```bash
echo '<STORAGE_USER_PASSWORD>' | sudo -S yum install -y "$PACKAGE"
```

Replace the password placeholder with the lab-provided password. Do not store real passwords in this GitHub documentation.

---

# Step 9: Execute Jenkins Job

Open:

```text
install-packages
```

Click:

```text
Build with Parameters
```

Enter:

```text
PACKAGE=vim-enhanced
```

Click Build.

---

# Step 10: Verify Jenkins Job

Open:

```text
install-packages → Build History → Console Output
```

Check the output for successful package installation.

---

# Step 11: Verify Package on Storage Server

Connect to the Storage Server:

```bash
ssh <storage-server-username>@<storage-server-hostname>
```

Check the installed package:

```bash
rpm -q vim-enhanced
```

To check whether the package is installed:

```bash
rpm -qa | grep vim-enhanced
```

---

# Useful Linux Commands

## Check Installed Package

```bash
rpm -q <package-name>
```

## Search Installed Packages

```bash
rpm -qa | grep <package-name>
```

## Check Package Manager

```bash
yum --version
```

## Check Current User

```bash
whoami
```

## Check Server Hostname

```bash
hostname
```

---

# Quick Command Summary

The main remote package installation command used in this challenge was:

```bash
echo '<STORAGE_USER_PASSWORD>' | sudo -S yum install -y "$PACKAGE"
```

Package verification:

```bash
rpm -q vim-enhanced
```

---

# Important Lesson

The most important concept in this challenge was **Jenkins Parameterized Builds with SSH Remote Execution**.

Instead of manually installing packages on the Storage Server, Jenkins can execute the installation command remotely.

The important parameter was:

```text
PACKAGE
```

The important Jenkins job was:

```text
install-packages
```

The main installation command was:

```bash
yum install -y "$PACKAGE"
```

This approach makes package installation reusable, flexible, and easier to automate.

**Day 71 completed successfully.** 🚀
