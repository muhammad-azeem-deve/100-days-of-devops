# Day 08 - Commands

This file contains the commands used during **Day 08 of my 100 Days of DevOps Challenge**.

The objective was to install **Ansible 4.7.0** using `pip3` and make it globally available for all users.

---

# Step 1: Install python3-pip

Install the Python 3 package manager:

```bash id="a1b2c3"
sudo dnf install python3-pip -y
```

---

# Step 2: Verify pip3

Check the installed `pip3` version:

```bash id="d4e5f6"
pip3 --version
```

Example output:

```text id="g7h8i9"
pip 20.x.x from /usr/lib/python3.x/site-packages/pip (python 3.x)
```

---

# Step 3: Check pip3 Location

```bash id="j1k2l3"
which pip3
```

This displays the location of the `pip3` executable.

---

# Step 4: Install Ansible 4.7.0 Globally

Install the required Ansible version:

```bash id="m4n5o6"
sudo pip3 install ansible==4.7.0
```

The `==` operator specifies the exact package version.

---

# Step 5: Verify Ansible Version

Check the installed Ansible version:

```bash id="p7q8r9"
ansible --version
```

This command displays the installed Ansible and Ansible Core versions.

---

# Step 6: Check Ansible Location

Verify where the Ansible executable is installed:

```bash id="s1t2u3"
which ansible
```

---

# Step 7: Check Ansible Package Information

Use `pip3 show` to display information about the installed package:

```bash id="v4w5x6"
pip3 show ansible
```

This displays details such as:

* Package name
* Version
* Installation location
* Dependencies

---

# Step 8: Verify Ansible from Another User

Since the requirement was to make Ansible **globally available for all users**, it can also be tested from another user account.

Switch to another user:

```bash id="y7z8a9"
su - <another-user>
```

Then run:

```bash id="b1c2d3"
ansible --version
```

If Ansible is available without installing it separately for that user, the global installation is working correctly.

---

# Quick Command Summary

```bash id="e4f5g6"
# Install pip3
sudo dnf install python3-pip -y

# Verify pip3
pip3 --version

# Check pip3 location
which pip3

# Install Ansible 4.7.0 globally
sudo pip3 install ansible==4.7.0

# Verify Ansible
ansible --version

# Check Ansible location
which ansible

# Check Ansible package information
pip3 show ansible

# Optional: Test from another user
su - <another-user>
ansible --version
```

---

# Understanding the Main Commands

## python3-pip

```bash id="h7i8j9"
sudo dnf install python3-pip -y
```

Installs the Python 3 package manager.

---

## Install Specific Ansible Version

```bash id="k1l2m3"
sudo pip3 install ansible==4.7.0
```

Here:

```text id="n4o5p6"
ansible
```

is the package name.

```text id="q7r8s9"
==
```

means install an exact version.

```text id="t1u2v3"
4.7.0
```

is the required Ansible version.

---

## Verify Ansible

```bash id="w4x5y6"
ansible --version
```

Displays the installed Ansible version and related environment information.

---

# Global Installation

The important installation command for this challenge was:

```bash id="z7a8b9"
sudo pip3 install ansible==4.7.0
```

Using `sudo` installs the package at the system level, making the Ansible executable available to users through the system `PATH`, rather than installing it only inside one user's home directory.

---

# Important Lesson

When a task requires software to be available to **all users**, it is important to perform a system-wide installation rather than installing the package only for the current user.

For this challenge, the key commands were:

```bash id="c1d2e3"
sudo dnf install python3-pip -y
```

and:

```bash id="f4g5h6"
sudo pip3 install ansible==4.7.0
```

Finally, the installation was verified using:

```bash id="i7j8k9"
ansible --version
```

**Day 08 completed successfully.** 
