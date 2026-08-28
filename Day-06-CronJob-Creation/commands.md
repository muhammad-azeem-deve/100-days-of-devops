# Day 06 - Commands

This file contains the commands used during **Day 06 of my 100 Days of DevOps Challenge**.

The objective was to install `cronie`, start the `crond` service, and create a root cron job that runs every 5 minutes on all required servers.

---

# Step 1: Access Server

Connect to each required server using SSH:

```bash
ssh <username>@<server-hostname>
```

Example:

```bash
ssh <username>@<server1-hostname>
```

Repeat the process for Server 2, Server 3, and any other required servers.

---

# Step 2: Verify the Server

Check the hostname:

```bash
hostname
```

This confirms that the correct server has been accessed.

---

# Step 3: Install cronie

Install the required cron package:

```bash
sudo dnf install cronie -y
```

The `cronie` package provides the cron daemon and required utilities.

---

# Step 4: Start crond

Start the cron service:

```bash
sudo systemctl start crond
```

---

# Step 5: Enable crond

Enable the service so that it starts automatically after reboot:

```bash
sudo systemctl enable crond
```

---

# Step 6: Check crond Status

Verify that the service is running:

```bash
sudo systemctl status crond
```

The service should show an active/running status.

---

# Step 7: Create Root Cron Job

Open the root user's crontab:

```bash
sudo crontab -e
```

Add a cron job that runs every 5 minutes:

```text
*/5 * * * * <command>
```

Save and exit the editor.

---

# Step 8: Verify Root Crontab

List the root user's cron jobs:

```bash
sudo crontab -l
```

Expected cron schedule:

```text
*/5 * * * * <command>
```

---

# Step 9: Verify Cron Service

Check the cron daemon again:

```bash
sudo systemctl status crond
```

---

# Step 10: Test the Cron Job

Wait for the next scheduled execution and verify that the command runs successfully.

The root crontab can be checked using:

```bash
sudo crontab -l
```

The cron service can be checked using:

```bash
sudo systemctl status crond
```

---

# Cron Expression

The five fields of a cron expression are:

```text
* * * * *
│ │ │ │ │
│ │ │ │ └── Day of week
│ │ │ └──── Month
│ │ └────── Day of month
│ └──────── Hour
└────────── Minute
```

For this challenge:

```text
*/5 * * * *
```

means:

```text
Every 5 minutes
```

---

# Quick Command Summary

```bash
# Access server
ssh <username>@<server-hostname>

# Verify server
hostname

# Install cron package
sudo dnf install cronie -y

# Start cron service
sudo systemctl start crond

# Enable cron service
sudo systemctl enable crond

# Check cron service
sudo systemctl status crond

# Edit root crontab
sudo crontab -e

# Add job that runs every 5 minutes
*/5 * * * * <command>

# List root cron jobs
sudo crontab -l
```

---

# Commands to Repeat on Every Server

## Server 1

```bash
ssh <username>@<server1-hostname>
hostname
sudo dnf install cronie -y
sudo systemctl start crond
sudo systemctl enable crond
sudo systemctl status crond
sudo crontab -e
sudo crontab -l
```

---

## Server 2

```bash
ssh <username>@<server2-hostname>
hostname
sudo dnf install cronie -y
sudo systemctl start crond
sudo systemctl enable crond
sudo systemctl status crond
sudo crontab -e
sudo crontab -l
```

---

## Server 3

```bash
ssh <username>@<server3-hostname>
hostname
sudo dnf install cronie -y
sudo systemctl start crond
sudo systemctl enable crond
sudo systemctl status crond
sudo crontab -e
sudo crontab -l
```

---

# Important Lesson

A cron job is associated with the user whose crontab contains it.

Since this challenge required the job to run as **root**, I created it using:

```bash
sudo crontab -e
```

rather than creating it in a normal user's crontab.

The important cron schedule for this challenge was:

```text
*/5 * * * * <command>
```

which runs the specified command **every 5 minutes**.

The same configuration must be applied to **all required servers** to successfully complete the challenge.
