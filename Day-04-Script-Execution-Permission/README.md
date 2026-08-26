# Day 04 - Script Execution Permission

## Challenge Overview

This is Day 04 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Linux File Permissions** and required giving **read and execute permissions to all users** on a shell script located on **Server 2**.

### Challenge Requirement

Access **Server 2** using SSH and give **read and execute permissions to all users** on the following file:

```text
/tmp/xfusioncorp.sh
```

After changing the permissions, the objective was to verify the permissions using the `ls -l` command.

---

## Objectives

The objectives of this challenge are:

* Access Server 2 using SSH.
* Verify that the correct server has been accessed.
* Locate the `/tmp/xfusioncorp.sh` script.
* Give read permission to all users.
* Give execute permission to all users.
* Verify the file permissions using `ls -l`.
* Understand Linux file permission notation.
* Understand how to use `chmod` to modify file permissions.

---

## Environment

| Item                 | Details               |
| -------------------- | --------------------- |
| Challenge            | 100 Days of DevOps    |
| Platform             | KodeKloud             |
| Day                  | 04                    |
| Server               | Server 2              |
| File                 | `/tmp/xfusioncorp.sh` |
| Required Permissions | Read and Execute      |
| Users                | All Users             |
| Status               | Completed             |

---

# Solution

## Step 1: Access Server 2

First, I accessed **Server 2** using SSH.

The SSH command format is:

```bash
ssh <username>@<server2-hostname>
```

Example:

```bash
ssh steve@stapp02
```

---

## Step 2: Verify the Server

After connecting to Server 2, I verified that I was working on the correct server.

I used:

```bash
hostname
```

This helped me confirm that I was connected to the required server before making any changes.

---

## Step 3: Check the Existing File Permissions

The required file was:

```text
/tmp/xfusioncorp.sh
```

I checked its existing permissions using:

```bash
ls -l /tmp/xfusioncorp.sh
```

This allowed me to see the current owner, group, and permission settings of the file.

---

## Step 4: Give Read and Execute Permissions to All Users

The requirement was to give **read and execute permissions to all users**.

I used the `chmod` command:

```bash
sudo chmod a+rx /tmp/xfusioncorp.sh
```

Here:

* `chmod` is used to change file permissions.
* `a` means **all users**.
* `+r` adds **read permission**.
* `+x` adds **execute permission**.

Therefore:

```text
a+rx
```

means:

```text
All users + Read + Execute
```

---

## Step 5: Verify the File Permissions

After changing the permissions, I verified the file using:

```bash
ls -l /tmp/xfusioncorp.sh
```

The output should show `r` and `x` permissions for the owner, group, and others.

For example:

```text
-r-xr-xr-x
```

This means:

* Owner: Read + Execute
* Group: Read + Execute
* Others: Read + Execute

The exact permission string may also contain write permission if it already existed, because `chmod a+rx` **adds** read and execute permissions without removing existing permissions.

---

# What I Learned

From this challenge, I learned and practiced:

* How to access a remote Linux server using SSH.
* How to verify the server using the `hostname` command.
* How Linux file permissions work.
* How to check file permissions using `ls -l`.
* How to use `chmod` to modify file permissions.
* What `r`, `w`, and `x` permissions represent.
* How the `a` option in `chmod` applies permissions to all users.
* How to give read and execute permissions to all users.
* The difference between owner, group, and other permissions.
* How to verify changes after modifying Linux file permissions.

# Challenge Status

Day 04 — Completed Successfully 

**Linux Permissions. One command. One more step forward in the DevOps journey.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
