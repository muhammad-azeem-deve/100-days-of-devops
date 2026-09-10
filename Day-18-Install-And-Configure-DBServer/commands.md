# Day 18 - MariaDB Commands

This file contains the commands used to complete **Day 18 of the KodeKloud 100 Days of DevOps Challenge**.

The task was to install MariaDB on the Nautilus DB Server, create a database and user, grant full permissions, and verify the configuration.

---

## 1. Connect to the DB Server

```bash
ssh <db-server-user>@<db-server-hostname>
```

Check the hostname:

```bash
hostname
```

---

## 2. Install MariaDB Server

```bash
sudo dnf install mariadb-server -y
```

---

## 3. Check MariaDB Service

```bash
sudo systemctl status mariadb
```

---

## 4. Start MariaDB

```bash
sudo systemctl start mariadb
```

---

## 5. Enable MariaDB at Boot

```bash
sudo systemctl enable mariadb
```

---

## 6. Access MariaDB as Root

```bash
sudo mysql -u root
```

---

## 7. Create the Database

Inside MariaDB:

```sql
CREATE DATABASE kodekloud_db8;
```

Verify:

```sql
SHOW DATABASES;
```

---

## 8. Create the Database User

```sql
CREATE USER 'kodekloud_pop'@'localhost' IDENTIFIED BY 'BruCStnMT5';
```

---

## 9. Grant Full Permissions

```sql
GRANT ALL PRIVILEGES ON kodekloud_db8.* TO 'kodekloud_pop'@'localhost';
```

---

## 10. Refresh Privileges

```sql
FLUSH PRIVILEGES;
```

---

## 11. Verify the User

```sql
SELECT User, Host FROM mysql.user;
```

Look for:

```text
kodekloud_pop
```

---

## 12. Verify User Permissions

```sql
SHOW GRANTS FOR 'kodekloud_pop'@'localhost';
```

The output should show privileges for:

```text
kodekloud_db8.*
```

---

## 13. Exit MariaDB

```sql
EXIT;
```

---

## 14. Test Login with the New User

```bash
mysql -u kodekloud_pop -p
```

Enter:

```text
BruCStnMT5
```

---

## 15. Verify Database Access

After logging in:

```sql
SHOW DATABASES;
```

The database should be visible:

```text
kodekloud_db8
```

---

# Complete Command Sequence

## Linux Commands

```bash
ssh <db-server-user>@<db-server-hostname>

hostname

sudo dnf install mariadb-server -y

sudo systemctl status mariadb

sudo systemctl start mariadb

sudo systemctl enable mariadb

sudo mysql -u root
```

## MariaDB Commands

```sql
CREATE DATABASE kodekloud_db8;

SHOW DATABASES;

CREATE USER 'kodekloud_pop'@'localhost' IDENTIFIED BY 'BruCStnMT5';

GRANT ALL PRIVILEGES ON kodekloud_db8.* TO 'kodekloud_pop'@'localhost';

FLUSH PRIVILEGES;

SELECT User, Host FROM mysql.user;

SHOW GRANTS FOR 'kodekloud_pop'@'localhost';

EXIT;
```

## Final Verification

```bash
mysql -u kodekloud_pop -p
```

Then:

```sql
SHOW DATABASES;
```

---

# Quick Summary

| Task               | Command                                                                   |
| ------------------ | ------------------------------------------------------------------------- |
| Install MariaDB    | `sudo dnf install mariadb-server -y`                                      |
| Start MariaDB      | `sudo systemctl start mariadb`                                            |
| Enable MariaDB     | `sudo systemctl enable mariadb`                                           |
| Login as root      | `sudo mysql -u root`                                                      |
| Create database    | `CREATE DATABASE kodekloud_db8;`                                          |
| Create user        | `CREATE USER 'kodekloud_pop'@'localhost' IDENTIFIED BY 'BruCStnMT5';`     |
| Grant permissions  | `GRANT ALL PRIVILEGES ON kodekloud_db8.* TO 'kodekloud_pop'@'localhost';` |
| Refresh privileges | `FLUSH PRIVILEGES;`                                                       |
| Check grants       | `SHOW GRANTS FOR 'kodekloud_pop'@'localhost';`                            |
| Test user          | `mysql -u kodekloud_pop -p`                                               |

---

# Important Lesson

The main lesson from this challenge was that installing a database server is only the first step. A proper database setup also requires:

**Install → Configure → Create Database → Create User → Grant Permissions → Verify**

This is an important basic workflow for database administration in a DevOps environment.
