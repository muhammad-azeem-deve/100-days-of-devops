# Day 09 - MariaDB Troubleshooting

## Challenge Overview

This is Day 09 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **MariaDB Troubleshooting**. The objective was to troubleshoot the MariaDB service, identify the reason why the service was not working correctly, fix the issue, and verify that MariaDB was running successfully.

During the troubleshooting process, I checked the MariaDB service status, restarted the service, inspected the service logs using `journalctl`, checked the MySQL data directory permissions, and finally corrected the ownership of the MariaDB runtime directory.

### Challenge Requirement

Troubleshoot the MariaDB service and make sure that the service is running successfully.

The troubleshooting process included:

* Starting the MariaDB service.
* Checking the service status.
* Restarting MariaDB.
* Checking MariaDB service logs.
* Checking the `/var/lib/mysql` directory.
* Checking the MariaDB logs again.
* Correcting the ownership of `/run/mariadb`.
* Restarting MariaDB.
* Verifying the final service status.

---

## Objectives

The objectives of this challenge are:

* Understand how to manage the MariaDB service.
* Start and stop system services using `systemctl`.
* Check the status of MariaDB.
* Restart a failed or problematic service.
* Read system logs using `journalctl`.
* Inspect MariaDB's data directory.
* Troubleshoot service permission issues.
* Correct directory ownership using `chown`.
* Verify that MariaDB is running successfully after troubleshooting.

---

## Environment

| Item              | Details            |
| ----------------- | ------------------ |
| Challenge         | 100 Days of DevOps |
| Platform          | KodeKloud          |
| Day               | 09                 |
| Service           | MariaDB            |
| Service Name      | `mariadb`          |
| Data Directory    | `/var/lib/mysql`   |
| Runtime Directory | `/run/mariadb`     |
| Required Owner    | `mysql:mysql`      |
| Main Tool         | `systemctl`        |
| Log Tool          | `journalctl`       |
| Status            | Completed          |

---

# Solution

## Step 1: Start MariaDB

First, I tried to start the MariaDB service using:

```bash
sudo systemctl start mariadb
```

This was the first step to check whether MariaDB could start normally.

---

## Step 2: Check MariaDB Status

After starting the service, I checked its status:

```bash
sudo systemctl status mariadb
```

The status output helped identify whether the service was running or whether there was an error preventing MariaDB from starting.

---

## Step 3: Restart MariaDB

I then tried restarting the MariaDB service:

```bash
sudo systemctl restart mariadb
```

After restarting, I checked the service status again.

---

## Step 4: Check MariaDB Logs

Since MariaDB was not working correctly, I checked the system logs for the MariaDB service:

```bash
sudo journalctl -xeu mariadb.service
```

The `journalctl` command was useful for finding detailed information about why the MariaDB service was failing.

---

## Step 5: Check the MySQL Data Directory

Next, I checked the permissions and ownership of the MariaDB data directory:

```bash
ls -l /var/lib/mysql
```

This helped me inspect the MariaDB files and directories and identify whether there were any ownership or permission-related problems.

---

## Step 6: Check MariaDB Logs Again

I checked the MariaDB service logs again to further investigate the issue:

```bash
sudo journalctl -xeu mariadb.service
```

After reviewing the troubleshooting information, I identified that the MariaDB runtime directory needed the correct ownership.

---

## Step 7: Correct the Runtime Directory Ownership

I changed the ownership of `/run/mariadb` to the `mysql` user and group:

```bash
sudo chown -R mysql:mysql /run/mariadb
```

Here:

```text
chown
```

is used to change file or directory ownership.

```text
-R
```

means the ownership change is applied recursively.

```text
mysql:mysql
```

sets:

```text
Owner → mysql
Group → mysql
```

The directory being corrected was:

```text
/run/mariadb
```

---

## Step 8: Restart MariaDB

After correcting the ownership, I restarted MariaDB:

```bash
sudo systemctl restart mariadb
```

This allowed MariaDB to start using the corrected runtime directory permissions.

---

## Step 9: Verify MariaDB Status

Finally, I checked the MariaDB service status:

```bash
sudo systemctl status mariadb
```

The service was successfully running after the troubleshooting and permission correction.

---

# Troubleshooting Process

The overall troubleshooting sequence was:

```text
Start MariaDB
      ↓
Check Status
      ↓
Restart MariaDB
      ↓
Check Service Logs
      ↓
Check /var/lib/mysql
      ↓
Check Service Logs Again
      ↓
Correct /run/mariadb Ownership
      ↓
Restart MariaDB
      ↓
Verify Status
```

The important troubleshooting command that helped identify the service issue was:

```bash
sudo journalctl -xeu mariadb.service
```

The final fix was:

```bash
sudo chown -R mysql:mysql /run/mariadb
```

---

# What I Learned

From this challenge, I learned and practiced:

* How to start MariaDB using `systemctl`.
* How to check the status of a Linux service.
* How to restart a service after making changes.
* How to troubleshoot service failures using `journalctl`.
* How to inspect the MariaDB data directory.
* How Linux ownership can affect services.
* How to use `chown` to change ownership.
* What the `mysql:mysql` ownership means.
* How the `-R` option works with `chown`.
* How to troubleshoot a service step by step instead of randomly changing configurations.
* The importance of checking service logs when a Linux service fails.

# Challenge Status

Day 09 — Completed Successfully 

**Troubleshooting is an important part of DevOps. Instead of just restarting the service again and again, I learned how to check logs, identify the issue, fix permissions, and verify the service properly.**

100 Days. 100 Challenges. One DevOps Journey.

I will continue documenting each challenge.
