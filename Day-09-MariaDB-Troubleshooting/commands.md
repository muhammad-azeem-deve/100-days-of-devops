# Day 09 - Commands

This file contains the commands used during **Day 09 of my 100 Days of DevOps Challenge**.

The objective was to troubleshoot the MariaDB service and make sure that it was running successfully.

---

# Step 1: Start MariaDB

Start the MariaDB service:

```bash
sudo systemctl start mariadb
```

---

# Step 2: Check MariaDB Status

Check the current status of the service:

```bash
sudo systemctl status mariadb
```

---

# Step 3: Restart MariaDB

Restart the MariaDB service:

```bash
sudo systemctl restart mariadb
```

---

# Step 4: Check MariaDB Service Logs

Check detailed MariaDB service logs:

```bash
sudo journalctl -xeu mariadb.service
```

This command helps identify errors that prevent MariaDB from starting or running correctly.

---

# Step 5: Check MySQL Data Directory

Check the ownership and permissions of the MariaDB data directory:

```bash
ls -l /var/lib/mysql
```

---

# Step 6: Check MariaDB Logs Again

Review the MariaDB service logs again:

```bash
sudo journalctl -xeu mariadb.service
```

---

# Step 7: Correct MariaDB Runtime Directory Ownership

Change the ownership of `/run/mariadb` to the `mysql` user and group:

```bash
sudo chown -R mysql:mysql /run/mariadb
```

### Meaning of the Command

```text
chown
```

Changes the owner and group of a file or directory.

```text
-R
```

Applies the ownership change recursively.

```text
mysql:mysql
```

Means:

```text
Owner → mysql
Group → mysql
```

Target directory:

```text
/run/mariadb
```

---

# Step 8: Restart MariaDB

After correcting the ownership:

```bash
sudo systemctl restart mariadb
```

---

# Step 9: Verify MariaDB Status

Finally, check the service status:

```bash
sudo systemctl status mariadb
```

The expected result is that the MariaDB service is:

```text
active (running)
```

---

# Quick Command Summary

```bash
# Start MariaDB
sudo systemctl start mariadb

# Check MariaDB status
sudo systemctl status mariadb

# Restart MariaDB
sudo systemctl restart mariadb

# Check MariaDB logs
sudo journalctl -xeu mariadb.service

# Check MySQL data directory
ls -l /var/lib/mysql

# Check MariaDB logs again
sudo journalctl -xeu mariadb.service

# Correct runtime directory ownership
sudo chown -R mysql:mysql /run/mariadb

# Restart MariaDB
sudo systemctl restart mariadb

# Final status check
sudo systemctl status mariadb
```

---



# Important Lesson

When a service fails, simply restarting it repeatedly is usually not enough.

A better troubleshooting approach is:

```text
Check Status
     ↓
Read Logs
     ↓
Inspect Files/Directories
     ↓
Identify the Cause
     ↓
Apply the Fix
     ↓
Restart Service
     ↓
Verify Status
```

For this challenge, the important troubleshooting tools were:

```bash
systemctl
journalctl
ls
chown
```

The final ownership correction was:

```bash
sudo chown -R mysql:mysql /run/mariadb
```

After restarting MariaDB, the service was successfully running.

**Day 09 completed successfully.**
