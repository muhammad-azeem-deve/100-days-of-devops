# Day 18 - MariaDB Database Server Setup

## Challenge Overview

This is **Day 18** of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was focused on setting up and configuring a **MariaDB database server** on the Nautilus DB Server in the Stratos Datacenter.

The task required installing MariaDB, creating a database and database user, and granting the user full permissions on the newly created database.

---

## Challenge Requirements

On the **Nautilus DB Server**, perform the following tasks:

1. Install and configure the **MariaDB server**.
2. Create a database named:
   `kodekloud_db8`
3. Create a database user:
   `kodekloud_pop`
4. Set the user's password to:
   `BruCStnMT5`
5. Grant **full permissions** to `kodekloud_pop` on the `kodekloud_db8` database.
6. Verify that the database, user, and permissions were configured correctly.

---

## Environment

| Component         | Details                            |
| ----------------- | ---------------------------------- |
| Challenge         | Day 18                             |
| Technology        | MariaDB                            |
| Server            | Nautilus DB Server                 |
| Database          | `kodekloud_db8`                    |
| Database User     | `kodekloud_pop`                    |
| Database Password | `BruCStnMT5`                       |
| Privileges        | Full privileges on `kodekloud_db8` |

---

## Solution

### Step 1 - Access the DB Server

First, I connected to the Nautilus DB Server using SSH.

```bash
ssh <db-server-user>@<db-server-hostname>
```

After connecting, I verified that I was working on the correct server.

```bash
hostname
```

---

### Step 2 - Install MariaDB Server

I installed the MariaDB server package on the DB Server.

```bash
sudo dnf install mariadb-server -y
```

After installation, I checked the MariaDB service.

```bash
sudo systemctl status mariadb
```

If the service was not running, I started it using:

```bash
sudo systemctl start mariadb
```

I also enabled MariaDB to start automatically after a server reboot:

```bash
sudo systemctl enable mariadb
```

---

### Step 3 - Access MariaDB

After installing MariaDB, I accessed the MariaDB database shell as the root user.

```bash
sudo mysql -u root
```

This opened the MariaDB command-line interface.

---

### Step 4 - Create the Database

Inside the MariaDB shell, I created the required database:

```sql
CREATE DATABASE kodekloud_db8;
```

I verified that the database was created successfully:

```sql
SHOW DATABASES;
```

The output included:

```text
kodekloud_db8
```

---

### Step 5 - Create the Database User

I created the required MariaDB user with the specified password:

```sql
CREATE USER 'kodekloud_pop'@'localhost' IDENTIFIED BY 'BruCStnMT5';
```

---

### Step 6 - Grant Full Permissions

I granted full privileges to the user on the `kodekloud_db8` database:

```sql
GRANT ALL PRIVILEGES ON kodekloud_db8.* TO 'kodekloud_pop'@'localhost';
```

Then I refreshed the privilege tables:

```sql
FLUSH PRIVILEGES;
```

---

### Step 7 - Verify User and Permissions

I verified that the user was created successfully:

```sql
SELECT User, Host FROM mysql.user;
```

Then I checked the privileges assigned to the user:

```sql
SHOW GRANTS FOR 'kodekloud_pop'@'localhost';
```

The output showed that `kodekloud_pop` had full privileges on:

```text
kodekloud_db8.*
```

---

### Step 8 - Verify Database Access

Finally, I tested that the newly created user could connect to MariaDB.

Exit the root MariaDB session:

```sql
EXIT;
```

Then connect using the new user:

```bash
mysql -u kodekloud_pop -p
```

Enter the password:

```text
BruCStnMT5
```

After logging in, I verified the database:

```sql
SHOW DATABASES;
```

The `kodekloud_db8` database was available to the user.

---

## Important Commands

### Install MariaDB

```bash
sudo dnf install mariadb-server -y
```

### Start MariaDB

```bash
sudo systemctl start mariadb
```

### Enable MariaDB

```bash
sudo systemctl enable mariadb
```

### Access MariaDB as root

```bash
sudo mysql -u root
```

### Create Database

```sql
CREATE DATABASE kodekloud_db8;
```

### Create User

```sql
CREATE USER 'kodekloud_pop'@'localhost' IDENTIFIED BY 'BruCStnMT5';
```

### Grant Permissions

```sql
GRANT ALL PRIVILEGES ON kodekloud_db8.* TO 'kodekloud_pop'@'localhost';
```

### Refresh Privileges

```sql
FLUSH PRIVILEGES;
```

### Verify Grants

```sql
SHOW GRANTS FOR 'kodekloud_pop'@'localhost';
```

---

## What I Learned

Through this challenge, I practiced:

* Installing MariaDB on a Linux server.
* Managing MariaDB using `systemctl`.
* Accessing MariaDB through the command line.
* Creating databases using SQL.
* Creating MariaDB users.
* Setting passwords for database users.
* Granting database-level privileges.
* Verifying user permissions using `SHOW GRANTS`.
* Testing database access with a newly created user.

This challenge helped me understand the basic administration tasks required when setting up a database server in a DevOps environment.

---

## Challenge Status

**Day 18: Completed Successfully**

MariaDB was installed and configured, the `kodekloud_db8` database was created, the `kodekloud_pop` user was created with the required password, and full privileges were granted and verified.
