# Day 74 - Commands

This file contains the commands used during **Day 74 of my 100 Days of DevOps Challenge**.

The objective was to automate MySQL database backups using Jenkins, SSH, `mysqldump`, and SCP.

---

# Step 1: Connect to Application Server

Connect to the Application Server:

```bash
ssh tony@stapp01
```

Verify the server:

```bash
hostname
```

---

# Step 2: Check MySQL Backup Utility

Verify that `mysqldump` is available:

```bash
which mysqldump
```

Check its version:

```bash
mysqldump --version
```

---

# Step 3: Generate the Backup Filename

Generate a filename using the current date:

```bash
date +%F
```

Create the backup filename:

```bash
BACKUP_FILE="db_$(date +%F).sql"
```

Example:

```text
db_2026-11-12.sql
```

---

# Step 4: Create Temporary Backup Directory

Create a directory to store the temporary database backup:

```bash
mkdir -p /tmp/db-backup
```

---

# Step 5: Create MySQL Database Dump

The general syntax is:

```bash
mysqldump -u <database-user> -p<database-password> <database-name> > backup.sql
```

For this challenge, the database details were:

```text
Database Name: kodekloud_db01
Database User: kodekloud_roy
```

The command using Jenkins credential variables was:

```bash
mysqldump -u "$DB_USER" -p"$DB_PASSWORD" kodekloud_db01 \
  > "/tmp/db-backup/db_$(date +%F).sql"
```

---

# Step 6: Create Destination Directory on Storage Server

Connect to the Storage Server:

```bash
ssh natasha@ststor01
```

Create the required backup directory:

```bash
mkdir -p /home/natasha/db_backups
```

Return to the Jenkins execution environment if necessary:

```bash
exit
```

---

# Step 7: Transfer Backup Using SCP

The general SCP syntax is:

```bash
scp <source-file> <username>@<server>:<destination-path>
```

For this challenge:

```bash
scp /tmp/db-backup/db_$(date +%F).sql \
  natasha@ststor01:/home/natasha/db_backups/
```

---

# Step 8: Verify Backup on Storage Server

Connect to the Storage Server:

```bash
ssh natasha@ststor01
```

Check the backup directory:

```bash
ls -lh /home/natasha/db_backups/
```

Check the specific backup file:

```bash
ls -lh /home/natasha/db_backups/db_$(date +%F).sql
```

Check that the backup file is not empty:

```bash
test -s /home/natasha/db_backups/db_$(date +%F).sql && echo "Backup exists and is not empty"
```

---

# Step 9: Jenkins Backup Script

The following script was configured as an **Execute shell** build step in the Jenkins Freestyle job.

The database credentials were supplied using Jenkins Credentials Binding, and SSH authentication was configured using the SSH Agent.

```bash
#!/bin/bash

set -e

# Define backup filename and directory
BACKUP_FILE="db_$(date +%F).sql"
BACKUP_DIR="/tmp/db-backup"

echo "Starting database backup..."

# Create temporary backup directory
mkdir -p "$BACKUP_DIR"

# Generate database backup from Application Server
ssh tony@stapp01 \
  "mysqldump -u '$DB_USER' -p'$DB_PASSWORD' kodekloud_db01" \
  > "$BACKUP_DIR/$BACKUP_FILE"

# Verify backup file
test -s "$BACKUP_DIR/$BACKUP_FILE"

echo "Database backup created successfully."

# Create destination directory on Storage Server
ssh natasha@ststor01 \
  "mkdir -p /home/natasha/db_backups"

# Transfer backup using SCP
scp "$BACKUP_DIR/$BACKUP_FILE" \
  natasha@ststor01:/home/natasha/db_backups/

echo "Backup transferred successfully."

# Verify backup on Storage Server
ssh natasha@ststor01 \
  "ls -lh /home/natasha/db_backups/$BACKUP_FILE"

echo "Database backup completed successfully."
```

---

# Step 10: Jenkins Job Schedule

The Jenkins job was scheduled using the following cron expression:

```text
*/10 * * * *
```

This runs the backup job every 10 minutes.

The schedule was configured inside:

```text
Jenkins Job → Configure → Build Triggers → Build periodically
```

---

# Step 11: Verify Jenkins Job

After saving the Jenkins job, I manually triggered it using:

```text
Build Now
```

I checked the execution logs under:

```text
database-backup → Build #1 → Console Output
```

The console output was used to verify the backup creation and file transfer.

---

# Quick Command Summary

```bash
# Connect to Application Server
ssh tony@stapp01

# Verify server
hostname

# Check mysqldump
which mysqldump
mysqldump --version

# Generate backup filename
date +%F

# Create temporary backup directory
mkdir -p /tmp/db-backup

# Create database backup
mysqldump -u "$DB_USER" -p"$DB_PASSWORD" kodekloud_db01 \
  > /tmp/db-backup/db_$(date +%F).sql

# Verify backup
ls -lh /tmp/db-backup/

# Create destination directory
ssh natasha@ststor01 \
  "mkdir -p /home/natasha/db_backups"

# Transfer backup
scp /tmp/db-backup/db_$(date +%F).sql \
  natasha@ststor01:/home/natasha/db_backups/

# Verify backup on Storage Server
ssh natasha@ststor01 \
  "ls -lh /home/natasha/db_backups/"
```

---

# Important Commands Explained

## mysqldump

```bash
mysqldump -u "$DB_USER" -p"$DB_PASSWORD" kodekloud_db01
```

Exports the MySQL database into SQL format.

## date +%F

```bash
date +%F
```

Returns the current date in `YYYY-MM-DD` format.

## SCP

```bash
scp backup.sql natasha@ststor01:/home/natasha/db_backups/
```

Transfers the backup file to the Storage Server.

## mkdir -p

```bash
mkdir -p /home/natasha/db_backups
```

Creates the backup directory if it does not already exist.

## Jenkins Cron Schedule

```text
*/10 * * * *
```

Runs the Jenkins job every 10 minutes.

---

# Important Lesson

The most important part of this challenge was understanding how Jenkins can automate database backups without requiring manual intervention.

The complete workflow involved:

1. Jenkins starts the scheduled job.
2. Jenkins supplies database credentials securely.
3. SSH connects to the Application Server.
4. `mysqldump` generates the database backup.
5. SCP transfers the backup to the Storage Server.
6. Jenkins verifies the backup file.

The key technologies used were:

```text
Jenkins
SSH
Credentials Binding
mysqldump
SCP
Cron Scheduling
```

**Day 74 completed successfully.** 🚀
