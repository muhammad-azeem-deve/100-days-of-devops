# Day 17 - Create PostgreSQL Database User and Grant Privileges

## Challenge Overview

This is **Day 17** of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge focused on **PostgreSQL database administration**. PostgreSQL was already installed on the Nautilus database server, and the task was to create a PostgreSQL database user, create a database, and grant the required permissions.

### Challenge Requirement

On the Nautilus database server:

1. Create a PostgreSQL user named `kodekloud_top`.
2. Set its password to `TmPcZjtRQx`.
3. Create a PostgreSQL database named `kodekloud_db6`.
4. Grant full privileges on the database to `kodekloud_top`.
5. Do **not** restart the PostgreSQL service.

---

## Environment

| Component                | Details                  |
| ------------------------ | ------------------------ |
| Server                   | Nautilus Database Server |
| Server Hostname          | `stdb01`                 |
| SSH User                 | `peter`                  |
| Database                 | PostgreSQL               |
| PostgreSQL User          | `kodekloud_top`          |
| PostgreSQL Database      | `kodekloud_db6`          |
| PostgreSQL User Password | `TmPcZjtRQx`             |

---

## Step 1 - Connect to the Database Server

I first connected to the Nautilus database server using SSH.

```bash
ssh peter@stdb01
```

---

## Step 2 - Verify PostgreSQL Installation

I checked the installed PostgreSQL version to confirm that PostgreSQL was already available.

```bash
psql -V
```

Since PostgreSQL was already installed, there was no need to install anything.

---

## Step 3 - Understand the Difference Between Linux and PostgreSQL Users

Initially, I created a Linux user using:

```bash
sudo useradd kodekloud_top
```

and changed its Linux password using:

```bash
sudo passwd kodekloud_top
```

However, after reviewing the task carefully, I realized that the requirement was to create a **PostgreSQL database user**, not a Linux operating-system user.

A Linux user and a PostgreSQL user are separate things.

Therefore, the important command for this task was:

```bash
sudo -u postgres psql -c "CREATE USER kodekloud_top WITH PASSWORD 'TmPcZjtRQx';"
```

This creates `kodekloud_top` as a PostgreSQL user and assigns the required password.

---

## Step 4 - Create the PostgreSQL Database

Next, I created the required database:

```bash
sudo -u postgres psql -c "CREATE DATABASE kodekloud_db6;"
```

This created the PostgreSQL database named `kodekloud_db6`.

---

## Step 5 - Grant Full Privileges

I then granted all privileges on the database to the PostgreSQL user:

```bash
sudo -u postgres psql -c "GRANT ALL PRIVILEGES ON DATABASE kodekloud_db6 TO kodekloud_top;"
```

This gives `kodekloud_top` full privileges on the `kodekloud_db6` database.

---

## Step 6 - Verify PostgreSQL Access

I also opened the PostgreSQL shell as the `postgres` administrative user:

```bash
sudo -u postgres psql
```

From the PostgreSQL shell, database and user information can be checked using commands such as:

```sql
\du
```

and:

```sql
\l
```

`\du` displays PostgreSQL roles/users, while `\l` displays available databases.

---

## Important Troubleshooting / Learning

### Linux User vs PostgreSQL User

One of the main things I learned from this task was the difference between:

* **Linux user** - created using `useradd`
* **PostgreSQL user/role** - created using PostgreSQL commands such as `CREATE USER`

Initially, I used:

```bash
sudo useradd kodekloud_top
```

But this was not what the task required.

The correct PostgreSQL user was created using:

```bash
sudo -u postgres psql -c "CREATE USER kodekloud_top WITH PASSWORD 'TmPcZjtRQx';"
```

The password specified in the PostgreSQL `CREATE USER` query is the database user's password. The Linux `passwd` command was therefore unnecessary for the actual database requirement.

### PostgreSQL Service

The task specifically stated:

> Do not try to restart PostgreSQL server service.

I did not restart the PostgreSQL service because it was already installed and running.

---

## Final Commands Used

The essential commands used to complete the task were:

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

## What I Learned

* How to work with an already-installed PostgreSQL server.
* How to create a PostgreSQL database user.
* How to set a PostgreSQL user's password.
* How to create a PostgreSQL database.
* How to grant database privileges to a PostgreSQL user.
* The difference between a Linux system user and a PostgreSQL database user.
* How to use `psql` through the `postgres` administrative account.
* Why unnecessary service restarts should be avoided when a task specifically says not to restart the service.

---

## Task Status

**Day 17 - Completed Successfully**

The PostgreSQL user `kodekloud_top` was created with the required password, the database `kodekloud_db6` was created, and full database privileges were granted to the user without restarting PostgreSQL.
