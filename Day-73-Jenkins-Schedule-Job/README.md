# Day 73 - Jenkins Scheduled Jobs

## Challenge Overview

This is Day 73 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Jenkins Scheduled Jobs and Automation**. The objective was to create a Jenkins job that automatically collects Apache log files from Application Server 2 and copies them to the Storage Server.

The job needed to execute automatically every 2 minutes without requiring manual execution.

### Challenge Requirement

Create a Jenkins job named `copy-logs` that periodically copies the following Apache log files from **Application Server 2** to the **Storage Server**:

- `access_log`
- `error_log`

The source directory was:

```text
/var/log/httpd/
```

The destination directory was:

```text
/usr/src/security/
```

The Jenkins job must execute every 2 minutes using the Build periodically trigger.

---

## Objectives

The objectives of this challenge are:

- Access the Jenkins dashboard.
- Log in to Jenkins.
- Create a Freestyle project named `copy-logs`.
- Configure a scheduled build using Jenkins Build Triggers.
- Understand Jenkins cron syntax.
- Schedule the job to run every 2 minutes.
- Configure an Execute Shell build step.
- Copy Apache access and error logs from App Server 2.
- Transfer the logs to the Storage Server using SCP.
- Verify the Jenkins build through Console Output.
- Verify the copied log files on the Storage Server.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 73 |
| Technology | Jenkins |
| Job Name | `copy-logs` |
| Job Type | Freestyle Project |
| Build Trigger | Build periodically |
| Schedule | Every 2 minutes |
| Source Server | App Server 2 (`stapp02`) |
| Source Directory | `/var/log/httpd/` |
| Source Files | `access_log`, `error_log` |
| Destination Server | Storage Server (`ststor01`) |
| Destination User | `natasha` |
| Destination Directory | `/usr/src/security/` |
| File Transfer | SCP |
| Status | Completed |

---

# Solution

## Step 1: Access Jenkins Dashboard

First, I opened the KodeKloud lab environment and clicked the **Jenkins** button from the top navigation bar.

I logged in using the credentials provided by the lab.

The published lab instructions provide these credentials:

```text
Username: admin
Password: Adm!n321
```

After successful authentication, I accessed the Jenkins dashboard.

---

## Step 2: Create a New Jenkins Job

From the Jenkins dashboard:

1. Clicked **New Item**.
2. Entered the job name:

```text
copy-logs
```

3. Selected **Freestyle project**.
4. Clicked **OK**.

This created a new Jenkins job named `copy-logs`.

---

## Step 3: Configure Build Triggers

After creating the job, I opened its configuration page.

I located the **Build Triggers** section and selected:

```text
Build periodically
```

In the Schedule field, I entered:

```text
*/2 * * * *
```

This configured Jenkins to execute the job automatically every 2 minutes.

### Understanding the Cron Expression

```text
*/2 * * * *
│   │ │ │ │
│   │ │ │ └── Day of week
│   │ │ └──── Month
│   │ └────── Day of month
│   └──────── Hour
└──────────── Minute
```

Explanation:

- `*/2` = Every 2 minutes.
- `*` = Every hour.
- `*` = Every day of the month.
- `*` = Every month.
- `*` = Every day of the week.

Therefore:

```text
*/2 * * * *
```

means the Jenkins job will run every 2 minutes.

---

## Step 4: Configure the Build Step

After configuring the schedule, I moved to the **Build** section.

I followed these steps:

1. Clicked **Add build step**.
2. Selected **Execute shell**.
3. Added the shell script to copy Apache log files to the Storage Server.

### Shell Script

```bash
#!/bin/bash

echo "Starting Apache log collection"

# Copy Apache access and error logs
sshpass -p '<STORAGE_PASSWORD>' scp \
-o StrictHostKeyChecking=no \
/var/log/httpd/access_log \
/var/log/httpd/error_log \
natasha@ststor01:/usr/src/security/

echo "Apache logs copied successfully"
```

I replaced `<STORAGE_PASSWORD>` with the password provided by the lab.

### Understanding the Script

- `#!/bin/bash` specifies the Bash interpreter.
- `echo` displays messages in the Jenkins console.
- `sshpass` supplies the SSH password.
- `scp` securely copies files between servers.
- `StrictHostKeyChecking=no` avoids an interactive SSH host-key confirmation.
- `/var/log/httpd/access_log` is the Apache access log.
- `/var/log/httpd/error_log` is the Apache error log.
- `natasha@ststor01` identifies the destination user and server.
- `/usr/src/security/` is the destination directory.

**Note:** The source log files must be accessible to the Jenkins build executor. In production, Jenkins Credentials should be used instead of hardcoding passwords.

---

## Step 5: Save the Jenkins Job

After configuring the build trigger and shell script:

1. Clicked **Save**.
2. Jenkins redirected me to the `copy-logs` job dashboard.
3. The job was now configured to execute periodically every 2 minutes.

---

## Step 6: Execute the Job Manually

To test the configuration immediately, I clicked:

```text
Build Now
```

This triggered the Jenkins job manually without waiting for the scheduled execution.

---

## Step 7: Verify the Jenkins Build

After executing the job, I checked the Build History section.

I opened the latest build and selected:

```text
Console Output
```

I checked the output to confirm that the SCP command executed successfully.

Expected successful output:

```text
Starting Apache log collection
Apache logs copied successfully
Finished: SUCCESS
```

The actual console output may also include the SCP command and connection details.

---

## Step 8: Verify Files on the Storage Server

Finally, I accessed the Storage Server using SSH:

```bash
ssh natasha@ststor01
```

I checked the destination directory:

```bash
ls -l /usr/src/security/
```

The directory should contain:

```text
access_log
error_log
```

This confirmed that the Apache log files were transferred to the required destination.

---

# Jenkins Scheduled Jobs

Jenkins scheduled jobs allow administrators to automate repetitive tasks without manually triggering builds.

For this challenge, I used the following scheduling expression:

```text
*/2 * * * *
```

Some other common Jenkins scheduling expressions are:

| Expression | Description |
|---|---|
| `*/2 * * * *` | Every 2 minutes |
| `*/5 * * * *` | Every 5 minutes |
| `*/10 * * * *` | Every 10 minutes |
| `0 * * * *` | Every hour |
| `0 0 * * *` | Every day at midnight |
| `0 9 * * 1` | Every Monday at 9 AM |

---

# What I Learned

From this challenge, I learned and practiced:

- How Jenkins scheduled jobs work.
- How to create a Freestyle project in Jenkins.
- How to configure Build periodically.
- How Jenkins uses cron expressions for scheduling.
- How to schedule a job to execute every 2 minutes.
- How to use Execute Shell in Jenkins.
- How to automate Apache log collection.
- How to use SCP for transferring files between servers.
- How to use `sshpass` for password-based authentication in a lab environment.
- How to execute a Jenkins job manually using Build Now.
- How to verify Jenkins builds using Console Output.
- How scheduled automation helps with regular log collection and troubleshooting.

# Challenge Status

Day 73 — Completed Successfully ✅

**Jenkins scheduled job configured to collect Apache access and error logs automatically every 2 minutes.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
