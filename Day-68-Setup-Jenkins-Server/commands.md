# Day 68 - Commands

This file contains the commands used during **Day 68 of my 100 Days of DevOps Challenge**.

The objective was to install Jenkins using APT, start the Jenkins service, and configure the administrator account through the Jenkins web interface.

---

## Step 1: Access Jenkins Server

Connect to the Jenkins server from the jump host:

```bash
ssh root@jenkins
```

Enter the password when prompted.

---

## Step 2: Verify Server

Check the hostname:

```bash
hostname
```

Check the current user:

```bash
whoami
```

Expected user:

```text
root
```

---

## Step 3: Update APT Package List

```bash
apt update
```

This updates the available package information.

---

## Step 4: Install Jenkins

Install Jenkins using APT:

```bash
apt install jenkins -y
```

---

## Step 5: Start Jenkins Service

Start Jenkins:

```bash
service jenkins start
```

---

## Step 6: Verify Jenkins Service

Check the Jenkins service status:

```bash
service jenkins status
```

Expected status:

```text
active (running)
```

---

## Step 7: Retrieve Initial Administrator Password

Run:

```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy the displayed password and paste it into the Jenkins browser setup page.

---

## Step 8: Jenkins Web Configuration

Complete the following steps through the Jenkins web interface:

1. Click the Jenkins button in the KodeKloud top navigation bar.
2. Enter the initial administrator password.
3. Click Continue.
4. Select Install suggested plugins.
5. Wait for plugin installation to complete.
6. Create the first administrator account.

### Administrator Details

```text
Username: theadmin
Password: Adm!n321
Full Name: Rose
Email: rose@jenkins.stratos.xfusioncorp.com
```

7. Click Save and Continue.
8. Keep the automatically populated Jenkins URL.
9. Click Save and Finish.
10. Click Start using Jenkins.

---

## Step 9: Final Verification

Check the Jenkins service:

```bash
service jenkins status
```

Check the initial administrator password file if required:

```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```

Check recent Jenkins logs:

```bash
tail -50 /var/log/jenkins/jenkins.log
```

---

# Troubleshooting Commands

If Jenkins fails to start or takes too long to start, use these commands.

### Check Service Status

```bash
service jenkins status
```

### Display Jenkins Logs

```bash
cat /var/log/jenkins/jenkins.log
```

### Display Last 50 Log Lines

```bash
tail -50 /var/log/jenkins/jenkins.log
```

### Follow Logs in Real Time

```bash
tail -f /var/log/jenkins/jenkins.log
```

Press `Ctrl + C` to stop following the logs.

---

# Quick Command Summary

```bash
# Connect to Jenkins server
ssh root@jenkins

# Verify server
hostname

# Verify current user
whoami

# Update APT package list
apt update

# Install Jenkins
apt install jenkins -y

# Start Jenkins
service jenkins start

# Check Jenkins status
service jenkins status

# Retrieve initial administrator password
cat /var/lib/jenkins/secrets/initialAdminPassword

# Check Jenkins logs
cat /var/log/jenkins/jenkins.log

# Display recent logs
tail -50 /var/log/jenkins/jenkins.log

# Follow Jenkins logs
tail -f /var/log/jenkins/jenkins.log
```

---

# Important Lesson

Jenkins is an automation server used for CI/CD pipelines.

The most important commands in this challenge were:

```bash
apt update
```

```bash
apt install jenkins -y
```

```bash
service jenkins start
```

```bash
service jenkins status
```

The initial administrator password was retrieved using:

```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```

**Remember:** The administrator account must be created through the Jenkins web interface using the exact details provided in the KodeKloud task.

Day 68 completed successfully! 🚀
