# Day 05 - SELinux Installation and Configuration

## Challenge Overview

This is Day 05 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **SELinux Installation and Configuration** and required installing SELinux on **Application Server 2** and changing its configuration from enforcing mode to disabled mode.

### Challenge Requirement

Access **Application Server 2** using SSH, install SELinux, and configure it to be **disabled**.

The configuration needed to be changed from:

```text
SELINUX=enforcing
```

to:

```text
SELINUX=disabled
```

The objective was to install SELinux, modify its configuration file, save the changes, and verify the final configuration.

---

## Objectives

The objectives of this challenge are:

* Access Application Server 2 using SSH.
* Verify that the correct server has been accessed.
* Install SELinux on the server.
* Open the SELinux configuration file.
* Change SELinux from enforcing to disabled.
* Save the updated configuration.
* Verify the SELinux configuration.
* Understand the purpose of the `/etc/selinux/config` file.
* Understand SELinux operating modes.
* Learn how to verify SELinux status using `sestatus`.

---

## Environment

| Item               | Details               |
| ------------------ | --------------------- |
| Challenge          | 100 Days of DevOps    |
| Platform           | KodeKloud             |
| Day                | 05                    |
| Server             | Application Server 2  |
| Configuration File | `/etc/selinux/config` |
| Initial Status     | `enforcing`           |
| Required Status    | `disabled`            |
| Verification       | `sestatus` / `grep`   |
| Status             | Completed             |

---

# Solution

## Step 1: Access Application Server 2

First, I accessed **Application Server 2** using SSH.

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

After connecting to the server, I verified that I was working on the correct server.

I used:

```bash
hostname
```

This helped me confirm that I was connected to **Application Server 2** before making any changes.

---

## Step 3: Install SELinux

Next, I installed the SELinux packages required by the server.

The installation command used was:

```bash
sudo yum install selinux-policy selinux-policy-targeted -y
```

This installs the SELinux policy packages on the server.

---

## Step 4: Open the SELinux Configuration File

After installing SELinux, I opened the SELinux configuration file:

```bash
sudo nano /etc/selinux/config
```

I found the following configuration:

```text
SELINUX=enforcing
```

and changed it to:

```text
SELINUX=disabled
```

Then I saved the file and exited the editor.

---

## Step 5: Verify the Configuration

After making the changes, I verified the configuration using:

```bash
cat /etc/selinux/config | grep SELINUX=
```

The expected result was:

```text
SELINUX=disabled
```

This confirmed that the SELinux configuration file had been updated correctly.

---

## Step 6: Check SELinux Status

I also used the `sestatus` command to check the current SELinux status:

```bash
sestatus
```

Depending on the current system state, the runtime status may not immediately reflect the configuration-file change until the server is rebooted.

The important configuration requirement was that `/etc/selinux/config` contained:

```text
SELINUX=disabled
```

---

# SELinux Modes

SELinux has three main modes:

### 1. Enforcing

In enforcing mode, SELinux actively enforces its security policies and blocks unauthorized actions.

```text
SELINUX=enforcing
```

### 2. Permissive

In permissive mode, SELinux does not block actions but logs policy violations.

```text
SELINUX=permissive
```

### 3. Disabled

In disabled mode, SELinux is turned off.

```text
SELINUX=disabled
```

For this challenge, the required configuration was:

```text
SELINUX=disabled
```

---

# What I Learned

From this challenge, I learned and practiced:

* How to access a Linux server using SSH.
* How to install SELinux packages.
* How to locate the SELinux configuration file.
* How to edit `/etc/selinux/config`.
* How to change SELinux from enforcing to disabled.
* How to verify SELinux configuration using `grep`.
* How to check SELinux status using `sestatus`.
* The difference between enforcing, permissive, and disabled modes.
* The importance of verifying configuration changes after editing system files.

# Challenge Status

Day 05 — Completed Successfully

**Another Linux security configuration completed and another step forward in my DevOps journey.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
