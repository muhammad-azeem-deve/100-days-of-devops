# Day 06 - Run a Cron Job as a Root User

## Challenge Overview

This is Day 06 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Linux Cron Jobs and Scheduled Tasks**. The task required creating a cron job that runs as the **root user** every 5 minutes on all the required servers.

To complete the task, I accessed each server one by one, installed the required cron package, started the `crond` service, created the cron job as the root user, and then verified that the cron job was configured correctly.

### Challenge Requirement

Create a cron job as the **root user** that runs **every 5 minutes** on all required servers.

The cron job needed to be configured on each server after installing the required cron package and starting the `crond` service.

---

## Objectives

The objectives of this challenge are:

* Access all required servers using SSH.
* Verify the correct server before making changes.
* Install the required cron package on each server.
* Start the `crond` service on each server.
* Create a cron job for the root user.
* Configure the cron job to run every 5 minutes.
* Verify the root user's crontab.
* Test and verify the cron job.
* Understand the five fields of a cron expression.
* Learn how Linux scheduled tasks work using `cron`.

---

## Environment

| Item      | Details              |
| --------- | -------------------- |
| Challenge | 100 Days of DevOps   |
| Platform  | KodeKloud            |
| Day       | 06                   |
| Servers   | All Required Servers |
| Package   | `cronie`             |
| Service   | `crond`              |
| User      | `root`               |
| Schedule  | Every 5 Minutes      |
| Status    | Completed            |

---

# Solution

## Step 1: Access the Servers

First, I accessed the required servers one by one using SSH.

The SSH command format is:

```bash
ssh <username>@<server-hostname>
```

For example:

```bash
ssh <username>@<server1-hostname>
```

I repeated the same process for all required servers.

---

## Step 2: Verify the Server

After connecting to each server, I verified the hostname to make sure I was working on the correct server.

```bash
hostname
```

This was important because the cron job had to be configured on **all required servers**.

---

## Step 3: Install the Cron Package

After accessing each server, I installed the required `cronie` package.

```bash
sudo dnf install cronie -y
```

The `cronie` package provides the cron daemon and utilities required to run scheduled tasks.

I performed this installation on each required server.

---

## Step 4: Start the crond Service

After installing `cronie`, I started the cron daemon:

```bash
sudo systemctl start crond
```

I also checked the service status:

```bash
sudo systemctl status crond
```

The service should be running successfully.

---

## Step 5: Enable crond at Boot

To make sure the cron service starts automatically after a server reboot, I enabled it:

```bash
sudo systemctl enable crond
```

This ensures that scheduled cron jobs can continue running after a system restart.

---

## Step 6: Create the Root Cron Job

The requirement was to create the cron job as the **root user**.

I opened the root user's crontab using:

```bash
sudo crontab -e
```

I added the required cron schedule:

```text
*/5 * * * * <command>
```

The `*/5` in the first field means that the command will run every 5 minutes.

---

## Step 7: Understand the Cron Expression

A cron schedule contains five time fields:

```text
* * * * *
│ │ │ │ │
│ │ │ │ └── Day of week
│ │ │ └──── Month
│ │ └────── Day of month
│ └──────── Hour
└────────── Minute
```

For a job that runs every 5 minutes:

```text
*/5 * * * *
```

This means:

```text
*/5 → Every 5 minutes
*   → Every hour
*   → Every day of the month
*   → Every month
*   → Every day of the week
```

Therefore:

```text
*/5 * * * * <command>
```

runs the specified command every 5 minutes.

---

## Step 8: Verify the Root Crontab

After saving the cron job, I verified the root user's crontab:

```bash
sudo crontab -l
```

The cron job should be displayed in the output.

For example:

```text
*/5 * * * * <command>
```

This confirms that the scheduled task has been added to the root user's crontab.

---

## Step 9: Test the Cron Job

To test the cron job, I waited for the scheduled execution time and checked whether the command was executed successfully.

The cron service can also be verified using:

```bash
sudo systemctl status crond
```

The service should show an active/running state.

I also verified the root crontab again:

```bash
sudo crontab -l
```

---

## Step 10: Repeat on All Servers

The complete process was repeated on every required server:

1. SSH into the server.
2. Verify the hostname.
3. Install `cronie`.
4. Start `crond`.
5. Enable `crond`.
6. Create the root cron job.
7. Verify the root crontab.
8. Test the scheduled task.

This was important because configuring the cron job on only one server would not satisfy the requirement.

---

# Cron Job Structure

The general format of a cron job is:

```text
minute hour day-of-month month day-of-week command
```

For example:

```text
*/5 * * * * <command>
```

means:

```text
Every 5 minutes → Run the specified command
```

---

# What I Learned

From this challenge, I learned and practiced:

* How cron jobs are used to schedule tasks in Linux.
* How to install the `cronie` package.
* How to start and manage the `crond` service.
* How to enable `crond` at system boot.
* How to create a cron job for the root user.
* How to use `crontab -e` to edit scheduled tasks.
* How to use `crontab -l` to list scheduled tasks.
* How cron expressions work.
* How to configure a job to run every 5 minutes.
* How to verify that the cron service is running.
* The importance of configuring the same task on all required servers.
* How scheduled automation can be used for repetitive Linux administration tasks.

# Challenge Status

Day 06 — Completed Successfully 

**Another Linux automation task completed. Learning cron jobs today and moving one step closer to becoming a better DevOps engineer.**

100 Days. 100 Challenges. One DevOps Journey.

I will continue documenting each challenge.
