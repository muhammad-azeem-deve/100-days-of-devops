# Day 74 - Jenkins Database Backup

## Challenge Overview

This is Day 74 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Jenkins Automation and Database Backup**. The objective was to automate a MySQL database backup using Jenkins, SSH, `mysqldump`, and SCP.

The task required creating a Jenkins job that connects to the Application Server, takes a backup of the MySQL database, and transfers the backup file to the Storage Server.

Jenkins Credentials Binding was used to securely supply the required database credentials during job execution.

### Challenge Requirement

Create a Jenkins job named `database-backup` that performs the following tasks:

1. Connect to the Application Server (`stapp01`).
2. Take a backup of the `kodekloud_db01` MySQL database.
3. Use the database user `kodekloud_roy`.
4. Generate the backup file using the format `db_$(date +%F).sql`.
5. Transfer the backup file to the Storage Server (`ststor01`).
6. Store the backup inside `/home/natasha/db_backups`.
7. Schedule the Jenkins job to run every 10 minutes.

---

## Objectives

The objectives of this challenge are:

- Understand Jenkins job automation.
- Create a Freestyle project in Jenkins.
- Install and configure the required Jenkins plugins.
- Configure SSH connectivity between servers.
- Use Jenkins Credentials Binding for secure credential management.
- Understand MySQL database backup using `mysqldump`.
- Transfer files between Linux servers using SCP.
- Generate date-based backup filenames.
- Schedule Jenkins jobs using cron expressions.
- Verify backup files on the Storage Server.
- Understand how CI/CD tools can automate database administration tasks.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 74 |
| Automation Tool | Jenkins |
| Job Name | `database-backup` |
| Job Type | Freestyle Project |
| Application Server | `stapp01` |
| Application User | `tony` |
| Storage Server | `ststor01` |
| Storage User | `natasha` |
| Database | `kodekloud_db01` |
| Database User | `kodekloud_roy` |
| Backup Tool | `mysqldump` |
| Transfer Tool | SCP |
| Backup Directory | `/home/natasha/db_backups` |
| Backup Filename | `db_$(date +%F).sql` |
| Schedule | `*/10 * * * *` |
| Status | Completed |

---

# Solution

## Step 1: Access Jenkins

First, I opened the Jenkins dashboard by clicking the Jenkins button in the KodeKloud environment.

I logged in using the provided lab credentials.

After successful authentication, I accessed the Jenkins dashboard.

---

## Step 2: Install Required Jenkins Plugins

Next, I opened:

```text
Manage Jenkins → Plugins
```

I checked and installed the required plugins:

- SSH Credentials
- Publish Over SSH
- Credentials Binding
- SSH Agent (for SSH key authentication in the build environment)

After installing the plugins, I restarted Jenkins when required.

---

## Step 3: Configure SSH Connection

I navigated to:

```text
Manage Jenkins → System
```

I located the **Publish over SSH** section and configured the Storage Server.

The configuration details were:

| Field | Value |
|---|---|
| Name | `ststor01` |
| Hostname | `ststor01` |
| Username | `natasha` |
| Remote Directory | `/home/natasha/db_backups` |

I configured the appropriate SSH credentials and tested the connection.

After successful testing, I saved the configuration.

---

## Step 4: Create Jenkins Credentials

Since the database required authentication, I configured the database credentials in Jenkins.

I used the Credentials Binding functionality to securely provide the database username and password to the build environment.

The environment variables were:

```text
DB_USER
DB_PASSWORD
```

The credentials were not intentionally stored as plain-text values in the build script.

I also configured SSH private-key credentials for accessing the Application and Storage Servers.

---

## Step 5: Create a Jenkins Freestyle Job

I returned to the Jenkins dashboard.

Then I followed these steps:

1. Clicked **New Item**.
2. Entered the job name `database-backup`.
3. Selected **Freestyle project**.
4. Clicked **OK**.

This created the Jenkins job.

---

## Step 6: Configure Build Periodically

Inside the job configuration, I opened the **Build Triggers** section.

I selected:

```text
Build periodically
```

Then I entered the required cron expression:

```text
*/10 * * * *
```

This schedules the database backup job to run every 10 minutes.

---

## Step 7: Configure the Database Backup Script

I configured the required credentials and SSH authentication in the Jenkins build environment.

Then I added an **Execute shell** build step.

The script performed the following operations:

1. Created a temporary backup directory.
2. Generated the backup filename using the current date.
3. Connected to the Application Server using SSH.
4. Executed `mysqldump` to export the MySQL database.
5. Verified that the backup file was created.
6. Created the destination directory on the Storage Server.
7. Transferred the backup using SCP.
8. Verified the transferred backup file.

The backup filename was generated using:

```bash
BACKUP_FILE="db_$(date +%F).sql"
```

The database dump was created using:

```bash
ssh tony@stapp01 \
  "mysqldump -u '$DB_USER' -p'$DB_PASSWORD' kodekloud_db01" \
  > "/tmp/db-backup/$BACKUP_FILE"
```

The backup was transferred using:

```bash
scp "/tmp/db-backup/$BACKUP_FILE" \
  natasha@ststor01:/home/natasha/db_backups/
```

---

## Step 8: Save and Execute the Jenkins Job

After configuring the job, I clicked **Save**.

Then I selected:

```text
Build Now
```

This manually triggered the Jenkins job.

I opened:

```text
database-backup → Build #1 → Console Output
```

The console output helped me verify the execution of the database dump and file transfer commands.

---

## Step 9: Verify the Backup on the Storage Server

After the Jenkins job completed, I accessed the Storage Server using SSH:

```bash
ssh natasha@ststor01
```

I checked the backup directory:

```bash
ls -lh /home/natasha/db_backups/
```

The backup file should follow this naming format:

```text
db_YYYY-MM-DD.sql
```

For example:

```text
db_2026-11-12.sql
```

This confirmed that the database backup was transferred to the required location.

---

# Understanding the Technologies

## 1. Jenkins

Jenkins is an automation server used to execute scheduled jobs and automate repetitive tasks.

In this challenge, Jenkins was responsible for running the database backup process automatically.

## 2. SSH

SSH stands for Secure Shell.

It provides secure remote access to Linux servers.

In this challenge, SSH was used to connect to the Application Server and Storage Server.

## 3. mysqldump

`mysqldump` is a command-line utility used to export MySQL databases.

It creates a SQL file containing database backup information.

Example:

```bash
mysqldump -u username -p database_name > backup.sql
```

## 4. SCP

SCP stands for Secure Copy Protocol.

It is used to securely transfer files between remote Linux systems.

Example:

```bash
scp backup.sql user@server:/path/
```

## 5. Credentials Binding

Jenkins Credentials Binding allows sensitive information to be provided to build steps through environment variables.

It helps avoid directly writing database credentials into build scripts.

## 6. Cron Expression

The schedule used in this challenge was:

```text
*/10 * * * *
```

It means:

```text
*/10 → Every 10 minutes
*    → Every hour
*    → Every day of the month
*    → Every month
*    → Every day of the week
```

---

# Backup Workflow

The complete process can be understood as:

```text
Jenkins Dashboard
       |
       v
Start database-backup Job
       |
       v
Load Jenkins Credentials
       |
       v
Connect to Application Server
       |
       v
Execute mysqldump
       |
       v
Create db_YYYY-MM-DD.sql
       |
       v
Connect to Storage Server
       |
       v
Transfer Backup Using SCP
       |
       v
Verify Backup File
       |
       v
Backup Completed
```

---

# What I Learned

From this challenge, I learned and practiced:

- How to create and configure Jenkins Freestyle projects.
- How Jenkins automates repetitive administrative tasks.
- How to install and configure Jenkins plugins.
- How to configure SSH connections in Jenkins.
- How to use Jenkins Credentials Binding.
- How to securely supply database credentials to build scripts.
- How to connect to remote servers using SSH.
- How to create MySQL database backups using `mysqldump`.
- How to generate filenames using Linux date commands.
- How to transfer backup files using SCP.
- How to schedule Jenkins jobs using cron expressions.
- How to verify database backups on a remote Storage Server.
- How Jenkins can be used for automated database administration and backup management.

# Challenge Status

Day 74 — Completed Successfully ✅

**Automated MySQL database backup using Jenkins, SSH, mysqldump, and SCP.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
