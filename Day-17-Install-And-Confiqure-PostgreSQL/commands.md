# Day 17 - PostgreSQL Database User and Privileges

This file contains the commands used for complition of task 17 of 100 days of devops with KodeKloud.

## 1. Connect to the Database Server

```bash
ssh peter@stdb01
```

Connects to the Nautilus database server.

---

## 2. Check PostgreSQL Version

```bash
psql -V
```

Confirms that PostgreSQL is installed.

---

## 3. Create PostgreSQL Database User

```bash
sudo -u postgres psql -c "CREATE USER kodekloud_top WITH PASSWORD 'TmPcZjtRQx';"
```

Creates the PostgreSQL user `kodekloud_top` and sets its database password.

> **Important:** This is the correct way to create the database user required by the task.

---

## 4. Create Database

```bash
sudo -u postgres psql -c "CREATE DATABASE kodekloud_db6;"
```

Creates the PostgreSQL database `kodekloud_db6`.

---

## 5. Grant Full Privileges

```bash
sudo -u postgres psql -c "GRANT ALL PRIVILEGES ON DATABASE kodekloud_db6 TO kodekloud_top;"
```

Grants all privileges on `kodekloud_db6` to `kodekloud_top`.

---

## 6. Open PostgreSQL Shell

```bash
sudo -u postgres psql
```

Opens the PostgreSQL interactive shell as the `postgres` administrative user.

---

## 7. Check PostgreSQL Users

Inside `psql`:

```sql
\du
```

Displays PostgreSQL users/roles.

---

## 8. Check Databases

Inside `psql`:

```sql
\l
```

Displays the PostgreSQL databases and their owners/access information.

---

# Commands That Were Initially Tried but Were Not Required

I initially used these commands:

```bash
sudo useradd kodekloud_top
sudo passwd kodekloud_top
```

These commands create and configure a **Linux operating-system user**, not a PostgreSQL database user.

After reviewing the task, I realized that the requirement was specifically for a **PostgreSQL user**.

Therefore, these commands were not necessary for completing the actual database requirement.

The correct PostgreSQL command was:

```bash
sudo -u postgres psql -c "CREATE USER kodekloud_top WITH PASSWORD 'TmPcZjtRQx';"
```

---

# Complete Command Sequence

```bash
ssh peter@stdb01

psql -V

sudo -u postgres psql -c "CREATE USER kodekloud_top WITH PASSWORD 'TmPcZjtRQx';"

sudo -u postgres psql -c "CREATE DATABASE kodekloud_db6;"

sudo -u postgres psql -c "GRANT ALL PRIVILEGES ON DATABASE kodekloud_db6 TO kodekloud_top;"

sudo -u postgres psql
```

Inside PostgreSQL:

```sql
\du
\l
```

---

# Important Lesson

**Linux users and PostgreSQL users are different.**

Linux user:

```bash
sudo useradd username
```

PostgreSQL user:

```sql
CREATE USER username WITH PASSWORD 'password';
```

For this challenge, the PostgreSQL user was required, so the database-level `CREATE USER` command was the important one.

---

# Final Result

* PostgreSQL user: `kodekloud_top` 
* Password configured: `TmPcZjtRQx` 
* Database: `kodekloud_db6` 
* Full database privileges granted: 
* PostgreSQL service restarted: **No** 
* Task completed successfully: **Yes** 
