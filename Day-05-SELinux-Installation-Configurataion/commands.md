# Day 05 - Commands

This file contains the commands used during **Day 05 of my 100 Days of DevOps Challenge**.

The objective was to install SELinux on **Application Server 2** and configure it to be disabled.

---

## Step 1: Access Application Server 2

Connect to Application Server 2 using SSH:

```bash
ssh <username>@<server2-hostname>
```

Example:

```bash
ssh steve@stapp02
```

---

## Step 2: Verify the Server

Check the hostname:

```bash
hostname
```

---

## Step 3: Install SELinux

Install the SELinux policy packages:

```bash
sudo yum install selinux-policy selinux-policy-targeted -y
```

---

## Step 4: Open SELinux Configuration

Open the SELinux configuration file:

```bash
sudo nano /etc/selinux/config
```

Find:

```text
SELINUX=enforcing
```

Change it to:

```text
SELINUX=disabled
```

Save and exit the file.

---

## Step 5: Verify SELinux Configuration

Check the configuration using:

```bash
cat /etc/selinux/config | grep SELINUX=
```

Expected output:

```text
SELINUX=disabled
```

---

## Step 6: Check SELinux Status

Use:

```bash
sestatus
```

This displays information about the current SELinux status and mode.

---

# Quick Command Summary

```bash
# Access Application Server 2
ssh <username>@<server2-hostname>

# Verify server
hostname

# Install SELinux packages
sudo yum install selinux-policy selinux-policy-targeted -y

# Open SELinux configuration
sudo nano /etc/selinux/config

# Change:
SELINUX=enforcing

# To:
SELINUX=disabled

# Verify configuration
cat /etc/selinux/config | grep SELINUX=

# Check SELinux status
sestatus
```

---

# SELinux Configuration

The main SELinux configuration file is:

```text
/etc/selinux/config
```

The required setting for this challenge was:

```text
SELINUX=disabled
```

---

# SELinux Modes

```text
Enforcing  → SELinux policies are actively enforced.
Permissive → Policy violations are logged but not blocked.
Disabled   → SELinux is disabled.
```

---

# Important Verification

The most important configuration verification command was:

```bash
cat /etc/selinux/config | grep SELINUX=
```

Expected output:

```text
SELINUX=disabled
```

The `sestatus` command can also be used to inspect the current SELinux runtime status.

If the configuration file has just been changed, the runtime status may require a reboot before it reflects the new setting.

---

# Important Lesson

When modifying important Linux security configurations, always:

1. Access the correct server.
2. Install the required packages.
3. Modify the correct configuration file.
4. Save the changes carefully.
5. Verify the configuration after editing.
6. Check the runtime status where applicable.

For this challenge, the key configuration was:

```text
SELINUX=disabled
```

and the main configuration file was:

```text
/etc/selinux/config
```
