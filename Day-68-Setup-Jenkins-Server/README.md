# Day 68 - Set Up Jenkins Server

## Challenge Overview

This is Day 68 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Jenkins Installation and Configuration**. The task required installing Jenkins on the Jenkins server using the APT package manager, starting the Jenkins service, and configuring an administrator account through the Jenkins web interface.

The main objective was to set up a working Jenkins server that could be used for CI/CD automation.

### Challenge Requirement

The task required:

- Access the Jenkins server from the jump host using SSH.
- Install Jenkins using the `apt` utility only.
- Start Jenkins using the `service` command.
- Verify that the Jenkins service is running.
- Access the Jenkins web interface using the Jenkins button in the KodeKloud interface.
- Unlock Jenkins using the initial administrator password.
- Install the suggested plugins.
- Create the administrator account with the required details.

---

## Objectives

The objectives of this challenge are:

- Access the Jenkins server using SSH.
- Verify the server connection.
- Update the APT package list.
- Install Jenkins using APT.
- Start the Jenkins service using `service`.
- Verify the Jenkins service status.
- Access the Jenkins web interface.
- Retrieve the initial administrator password.
- Install suggested Jenkins plugins.
- Create the required administrator account.
- Understand the basic purpose of Jenkins in CI/CD.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 68 |
| Server | Jenkins Server |
| Access Method | SSH |
| Package Manager | APT |
| Software | Jenkins |
| Service | `jenkins` |
| Web Interface | Jenkins Dashboard |
| Administrator Username | `theadmin` |
| Administrator Full Name | `Rose` |
| Administrator Email | `rose@jenkins.stratos.xfusioncorp.com` |
| Status | Completed |

---

# Solution

## Step 1: Access the Jenkins Server

First, I accessed the Jenkins server from the jump host using the root user.

I used:

```bash
ssh root@jenkins
```

When prompted, I entered the password provided by the KodeKloud lab.

---

## Step 2: Verify the Server

After connecting to the Jenkins server, I verified the hostname:

```bash
hostname
```

This confirmed that I was working on the correct server.

---

## Step 3: Update the APT Package List

Before installing Jenkins, I updated the package repository information:

```bash
apt update
```

This command refreshes the package information available through the APT package manager.

---

## Step 4: Install Jenkins

Next, I installed Jenkins using the APT package manager:

```bash
apt install jenkins -y
```

Here:

- `apt` is the package management utility.
- `install` installs the required package.
- `jenkins` is the package name.
- `-y` automatically confirms the installation.

This installed Jenkins on the server.

---

## Step 5: Start Jenkins Service

After installation, I started the Jenkins service using the required `service` command:

```bash
service jenkins start
```

This starts the Jenkins server in the background.

---

## Step 6: Verify Jenkins Service Status

After starting Jenkins, I checked the service status:

```bash
service jenkins status
```

The service should show an active/running status.

This confirmed that Jenkins was running on the server.

---

## Step 7: Access Jenkins Web Interface

After successfully installing Jenkins:

1. I returned to the KodeKloud lab interface.
2. I clicked the **Jenkins** button in the top navigation bar.
3. The Jenkins web interface opened in a new browser tab.
4. Jenkins displayed the initial setup page and requested an administrator password.

---

## Step 8: Retrieve the Initial Administrator Password

To unlock Jenkins, I returned to the server terminal and executed:

```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```

This displayed the initial administrator password.

I copied the password and entered it into the Jenkins setup page.

Then I clicked **Continue**.

---

## Step 9: Install Suggested Plugins

After unlocking Jenkins, the setup wizard displayed two options for plugin installation.

I selected:

**Install suggested plugins**

Jenkins automatically downloaded and installed the recommended plugins.

I waited until the installation process was completed.

---

## Step 10: Create the Administrator Account

After installing the suggested plugins, Jenkins asked me to create the first administrator account.

I entered the details provided in the challenge:

| Field | Value |
|------|-------|
| Username | `theadmin` |
| Password | `Adm!n321` |
| Full Name | `Rose` |
| Email | `rose@jenkins.stratos.xfusioncorp.com` |

After entering the information, I clicked **Save and Continue**.

---

## Step 11: Complete Jenkins Configuration

Next, Jenkins displayed the instance configuration page.

I kept the automatically populated Jenkins URL and clicked:

**Save and Finish**

After that, I clicked **Start using Jenkins**.

The Jenkins dashboard appeared, confirming that the initial configuration was completed.

---

## Step 12: Final Verification

Finally, I returned to the terminal and verified the Jenkins service:

```bash
service jenkins status
```

I also verified the setup by logging into the Jenkins dashboard using the administrator account.

The Jenkins server was successfully installed and configured.

---

# Troubleshooting

If Jenkins takes too long to start or the terminal reports a timeout, I can investigate the issue using the following commands.

### 1. Check Jenkins Service Status

```bash
service jenkins status
```

This displays the current status of the Jenkins service.

### 2. Check Jenkins Logs

```bash
cat /var/log/jenkins/jenkins.log
```

This displays the Jenkins log file and helps identify startup errors.

### 3. Check Recent Logs

```bash
tail -50 /var/log/jenkins/jenkins.log
```

This displays the last 50 lines of the Jenkins log file.

These commands help identify problems such as Java errors, port conflicts, or other startup issues.

---

# What I Learned

From this challenge, I learned and practiced:

- How to access a remote Linux server using SSH.
- How to install software using the APT package manager.
- How to install Jenkins on a Linux server.
- How to start services using the `service` command.
- How to verify service status.
- How to access the Jenkins web interface.
- How to retrieve the initial Jenkins administrator password.
- How to install suggested Jenkins plugins.
- How to create the first administrator account.
- How Jenkins is used in CI/CD automation.
- How to troubleshoot Jenkins startup problems using log files.
- The importance of verifying both the Jenkins service and web interface.

# Challenge Status

Day 68 — Completed Successfully ✅

**Jenkins server installed, service started, and administrator account configured successfully.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
