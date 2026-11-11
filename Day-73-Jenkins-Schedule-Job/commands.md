# Day 73 - Commands

This file contains the commands and shell script used during **Day 73 of my 100 Days of DevOps Challenge**.

The objective was to create a Jenkins scheduled job named `copy-logs` that automatically copies Apache logs from App Server 2 to the Storage Server every 2 minutes.

---

# Step 1: Access Jenkins

Open Jenkins from the KodeKloud lab environment and log in using the provided credentials.

```text
Username: admin
Password: <JENKINS_PASSWORD>
```

---

# Step 2: Create Jenkins Job

From the Jenkins dashboard:

```text
New Item → copy-logs → Freestyle project → OK
```

---

# Step 3: Configure Build Trigger

Navigate to:

```text
Configure → Build Triggers → Build periodically
```

Enter the following cron expression:

```text
*/2 * * * *
```

This schedules the Jenkins job to run every 2 minutes.

---

# Step 4: Configure Execute Shell

Navigate to:

```text
Build → Add build step → Execute shell
```

Add the following script:

```bash
#!/bin/bash

echo "Starting Apache log collection"

sshpass -p '<STORAGE_PASSWORD>' scp \
-o StrictHostKeyChecking=no \
/var/log/httpd/access_log \
/var/log/httpd/error_log \
natasha@ststor01:/usr/src/security/

echo "Apache logs copied successfully"
```

Replace `<STORAGE_PASSWORD>` with the actual Storage Server password provided by the lab.

---

# Step 5: Save and Execute the Job

Save the Jenkins configuration.

From the job dashboard, click:

```text
Build Now
```

This manually triggers the job for testing.

---

# Step 6: Verify Jenkins Build

Open:

```text
Build History → Latest Build → Console Output
```

Check whether the SCP operation completed successfully.

---

# Step 7: Access Storage Server

Connect to the Storage Server:

```bash
ssh natasha@ststor01
```

---

# Step 8: Verify Copied Log Files

List the destination directory:

```bash
ls -l /usr/src/security/
```

Check the access log:

```bash
ls -l /usr/src/security/access_log
```

Check the error log:

```bash
ls -l /usr/src/security/error_log
```

Both files should be present.

---

# Important Commands

## Jenkins Cron Expression

```text
*/2 * * * *
```

Runs the Jenkins job every 2 minutes.

## SCP Command

```bash
scp source_file username@server:destination_directory
```

Copies a file from one machine to another using SSH.

## SSH Command

```bash
ssh natasha@ststor01
```

Connects to the Storage Server.

## List Files

```bash
ls -l /usr/src/security/
```

Displays the copied Apache log files.

---

# Complete Shell Script

The following script was used in the Jenkins Execute Shell build step:

```bash
#!/bin/bash

# Display job information
echo "=================================="
echo "Jenkins Apache Log Collection"
echo "=================================="

# Copy Apache logs to Storage Server
sshpass -p '<STORAGE_PASSWORD>' scp \
-o StrictHostKeyChecking=no \
/var/log/httpd/access_log \
/var/log/httpd/error_log \
natasha@ststor01:/usr/src/security/

# Display completion message
echo "Apache logs copied successfully"
```

**Important:** For reliable error reporting, use `set -e` at the beginning of the script so that a failed SCP command does not incorrectly produce a successful completion message.

---

# Quick Command Summary

```bash
# Access Jenkins
# Open Jenkins UI from KodeKloud

# Create Freestyle project
# Job Name: copy-logs

# Configure Build periodically
*/2 * * * *

# Add Execute Shell script
#!/bin/bash

sshpass -p '<STORAGE_PASSWORD>' scp \
-o StrictHostKeyChecking=no \
/var/log/httpd/access_log \
/var/log/httpd/error_log \
natasha@ststor01:/usr/src/security/

# Save the job
# Click Build Now

# Verify Console Output
# Build History → Latest Build → Console Output

# Access Storage Server
ssh natasha@ststor01

# Verify copied files
ls -l /usr/src/security/
```

---

# Important Lesson

Jenkins scheduled jobs help automate repetitive tasks by executing jobs at predefined intervals.

For this challenge, the most important configuration was:

**Job Name:**

```text
copy-logs
```

**Schedule:**

```text
*/2 * * * *
```

**Source Files:**

```text
/var/log/httpd/access_log
/var/log/httpd/error_log
```

**Destination:**

```text
natasha@ststor01:/usr/src/security/
```

The main concepts practiced in this challenge were:

- Jenkins Freestyle Projects
- Build Triggers
- Cron Expressions
- Execute Shell
- SSH and SCP
- Automated Log Collection

**Day 73 completed successfully.** 🚀
