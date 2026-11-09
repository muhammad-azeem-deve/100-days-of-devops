# Day 71 - Configure Jenkins Job for Package Installation

## Challenge Overview

This is Day 71 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Jenkins Job Configuration and SSH-Based Remote Execution**. The task required creating a parameterized Jenkins job that could install packages on the Storage Server automatically.

The objective was to configure Jenkins with the required SSH plugin, add Storage Server credentials, configure the remote server connection, create a parameterized job, and execute the job to install a package.

### Challenge Requirement

Create a Jenkins job named `install-packages` with the following specifications:

- Add a String Parameter named `PACKAGE`.
- Configure the job to install the package specified by the `$PACKAGE` parameter.
- Execute the package installation on the Storage Server.
- Run the Jenkins job at least once.
- Verify that the package was successfully installed.

---

## Objectives

The objectives of this challenge are:

- Access the Jenkins dashboard.
- Install the required SSH-related Jenkins plugin.
- Configure SSH credentials in Jenkins.
- Connect Jenkins to the Storage Server.
- Configure the Storage Server as a Jenkins remote host.
- Understand Jenkins nodes and agents.
- Create a Freestyle Jenkins job.
- Enable parameterized builds.
- Add a String Parameter named `PACKAGE`.
- Execute remote shell commands through SSH.
- Install packages dynamically using Jenkins.
- Verify successful job execution through Console Output.
- Understand how Jenkins automates remote server administration.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 71 |
| Tool | Jenkins |
| Job Name | `install-packages` |
| Job Type | Freestyle Project |
| Plugin | SSH Plugin |
| Authentication | SSH Credentials |
| Target Server | Storage Server |
| Parameter | `PACKAGE` |
| Parameter Type | String |
| Example Package | `vim-enhanced` |
| Execution Method | SSH Remote Execution |
| Status | Completed |

---

# Solution

## Step 1: Access Jenkins Dashboard

First, I accessed the Jenkins dashboard using the Jenkins button available in the KodeKloud lab environment.

I logged in using the Jenkins administrator credentials provided by the lab.

After successful login, I opened the Jenkins dashboard.

---

## Step 2: Install the SSH Plugin

Next, I installed the required SSH plugin to allow Jenkins to execute commands on remote servers.

I followed these steps:

1. Open Manage Jenkins.
2. Select Plugins.
3. Open Available Plugins.
4. Search for the SSH plugin.
5. Select the required plugin.
6. Click Install.

After installation, I selected:

`Restart Jenkins when installation is complete and no jobs are running`

I waited for Jenkins to restart and refreshed the dashboard.

The SSH plugin provides the functionality required to execute shell scripts on remote hosts through SSH.

---

## Step 3: Add Storage Server Credentials

After installing the plugin, I configured the Storage Server credentials in Jenkins.

I navigated to:

`Manage Jenkins → Credentials → System → Global Credentials`

Then I clicked **Add Credentials**.

I configured the following details:

| Field | Value |
|------|-------|
| Kind | Username with password |
| Username | Storage Server username |
| Password | Storage Server password |
| ID | `storage` |
| Description | Storage Server SSH Credentials |

After entering the required details, I saved the credentials.

These credentials allow Jenkins to authenticate with the Storage Server.

---

## Step 4: Configure Storage Server SSH Connection

Next, I configured the remote SSH connection.

I navigated to:

`Manage Jenkins → System`

I located the **SSH Remote Hosts** section and added a new SSH connection.

I configured:

| Field | Value |
|------|-------|
| Hostname | Storage Server hostname |
| Port | `22` |
| Credentials | Storage Server credentials |

After configuring the connection, I clicked **Check Connection**.

The connection was successfully verified.

Then I saved the configuration.

---

## Step 5: Configure Storage Server as a Jenkins Node

I also worked with Jenkins nodes to understand how remote machines can be connected to Jenkins.

I navigated to:

`Manage Jenkins → Nodes → New Node`

I entered a name for the Storage Server node and selected **Permanent Agent**.

I configured the required node information, including:

- Node name.
- Remote root directory.
- Labels.
- SSH connection details.
- Authentication credentials.
- SSH launch method.

After saving the configuration, I checked the node connection status.

**Note:** A Jenkins SSH agent node and an SSH Remote Host are separate configurations. The SSH Remote Host is the connection used by the standard Day 71 remote execution build step.

---

## Step 6: Create a New Jenkins Job

After configuring the SSH connection, I created a new Jenkins job.

I followed these steps:

1. Open the Jenkins Dashboard.
2. Click New Item.
3. Enter the job name:

```text
install-packages
```

4. Select Freestyle Project.
5. Click OK.

This created the Jenkins job.

---

## Step 7: Enable Parameterized Build

Inside the job configuration, I enabled:

`This project is parameterized`

Then I added a **String Parameter**.

I configured the parameter as follows:

| Field | Value |
|------|-------|
| Name | `PACKAGE` |
| Type | String Parameter |
| Default Value | `vim-enhanced` |
| Description | Package name to install on Storage Server |

The `PACKAGE` parameter allows the user to specify which package should be installed whenever the Jenkins job runs.

For example:

```text
PACKAGE=vim-enhanced
```

---

## Step 8: Configure Remote Package Installation

Next, I configured the build step.

Under Build Steps, I selected:

`Execute shell script on remote host using SSH`

I selected the configured Storage Server SSH connection.

I added the package installation command:

```bash
echo '<STORAGE_USER_PASSWORD>' | sudo -S yum install -y "$PACKAGE"
```

Here:

- `echo` supplies the password to standard input.
- `sudo -S` allows sudo to read the password from standard input.
- `yum install` installs the requested package.
- `-y` automatically confirms the installation.
- `$PACKAGE` uses the value supplied through the Jenkins parameter.

The password placeholder must be replaced with the lab-provided password during configuration. It should not be committed to GitHub.

---

## Step 9: Save and Execute the Jenkins Job

After completing the configuration, I saved the Jenkins job.

I opened the `install-packages` job and selected:

`Build with Parameters`

I entered the package name:

```text
vim-enhanced
```

Then I clicked Build.

Jenkins connected to the Storage Server through SSH and executed the package installation command.

---

## Step 10: Verify Job Execution

After running the job, I opened:

`install-packages → Build History → Console Output`

I checked the output to confirm that the package installation command executed successfully.

I also verified the package directly on the Storage Server using:

```bash
rpm -q vim-enhanced
```

If the package information was displayed, it confirmed that the installation was successful.

---

# Understanding Jenkins Parameters

A Jenkins parameter allows users to provide input whenever they execute a job.

For this challenge, the parameter was:

```text
PACKAGE
```

For example, different package names can be supplied:

```text
vim-enhanced
git
wget
curl
```

The same Jenkins job can install different packages without modifying its configuration every time.

This makes the job reusable and flexible.

---

# Understanding Jenkins SSH Credentials

Jenkins credentials are used to securely store authentication information.

Instead of repeatedly entering the Storage Server username and password, Jenkins can use the saved credentials when establishing an SSH connection.

The credentials configured in this challenge were:

```text
Username with password
```

The saved credential was then selected in the SSH Remote Host configuration.

---

# What I Learned

From this challenge, I learned and practiced:

- How Jenkins connects to remote Linux servers.
- How to install SSH-related Jenkins plugins.
- How to configure username and password credentials.
- How to configure SSH Remote Hosts.
- How Jenkins nodes and agents work.
- How to create a Freestyle Jenkins project.
- How to enable parameterized builds.
- How to create String Parameters.
- How to use `$PACKAGE` inside a Jenkins build.
- How to execute shell commands on remote servers using SSH.
- How to automate package installation through Jenkins.
- How to verify job execution using Console Output.
- How Jenkins reduces manual server administration work.

# Challenge Status

Day 71 — Completed Successfully ✅

**Configured a parameterized Jenkins job to automate package installation on the Storage Server through SSH.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
