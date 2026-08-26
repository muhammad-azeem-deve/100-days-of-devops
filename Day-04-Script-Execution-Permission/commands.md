# Day 04 - Commands

This file contains the commands used during **Day 04 of my 100 Days of DevOps Challenge**.

The objective was to give **read and execute permissions to all users** on `/tmp/xfusioncorp.sh` located on Server 2.

---

## Step 1: Access Server 2

Connect to Server 2 using SSH:

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

This confirms that I am working on the correct server.

---

## Step 3: Check Existing File Permissions

Check the current permissions of the script:

```bash
ls -l /tmp/xfusioncorp.sh
```

---

## Step 4: Give Read and Execute Permissions to All Users

Use `chmod`:

```bash
sudo chmod a+rx /tmp/xfusioncorp.sh
```

### Meaning of the Command

```text
chmod
```

Changes file permissions.

```text
a
```

Means **all users**.

```text
+r
```

Adds **read permission**.

```text
+x
```

Adds **execute permission**.

Therefore:

```text
a+rx
```

means:

```text
Give read and execute permissions to all users.
```

---

## Step 5: Verify the Permissions

Run:

```bash
ls -l /tmp/xfusioncorp.sh
```

Expected permission pattern:

```text
-r-xr-xr-x
```

This represents:

```text
Owner  → Read + Execute
Group  → Read + Execute
Others → Read + Execute
```

If the file already had other permissions, the output may contain additional permission bits.

---

# Quick Command Summary

```bash
# Access Server 2
ssh <username>@<server2-hostname>

# Verify server
hostname

# Check existing permissions
ls -l /tmp/xfusioncorp.sh

# Give read and execute permissions to all users
sudo chmod a+rx /tmp/xfusioncorp.sh

# Verify final permissions
ls -l /tmp/xfusioncorp.sh
```

---

# Understanding Linux Permissions

Linux permissions are divided into three categories:

```text
User/Owner
Group
Others
```

The three basic permissions are:

```text
r = Read
w = Write
x = Execute
```

For example:

```text
-r-xr-xr-x
```

can be understood as:

```text
Owner  → r-x
Group  → r-x
Others → r-x
```

So all three categories have:

```text
Read + Execute
```

---

# Important Lesson

When working with Linux file permissions, always verify the file permissions **before and after** making changes.

The `ls -l` command is useful for checking the current permission settings, while `chmod` is used to modify them.

For this challenge, the important command was:

```bash
sudo chmod a+rx /tmp/xfusioncorp.sh
```

This gave all users the required **read and execute permissions** on the script.
