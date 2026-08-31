# Day 08 - Install Ansible Globally

## Challenge Overview

This is Day 08 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Ansible Installation and Configuration**. The task required installing Ansible version **4.7.0** using `pip3` and making sure that Ansible was **globally available for all users** on the Linux server.

To complete the challenge, I first installed `python3-pip`, verified the installation, installed the required version of Ansible using `pip3`, and finally verified that Ansible was installed successfully and available system-wide.

### Challenge Requirement

Install **Ansible version 4.7.0** on the Linux server using `pip3`.

The installation must be **globally available for all users** on the server.

---

## Objectives

The objectives of this challenge are:

* Install `python3-pip` on the Linux server.
* Verify that `pip3` was installed successfully.
* Install Ansible version `4.7.0` using `pip3`.
* Install Ansible globally so that it is available to all users.
* Verify the installed Ansible version.
* Understand how Python packages can be installed using `pip3`.
* Understand the difference between user-level and system-wide package installation.

---

## Environment

| Item             | Details            |
| ---------------- | ------------------ |
| Challenge        | 100 Days of DevOps |
| Platform         | KodeKloud          |
| Day              | 08                 |
| Operating System | Linux              |
| Package Manager  | `pip3`             |
| Python Package   | Ansible            |
| Ansible Version  | `4.7.0`            |
| Installation     | Global             |
| Availability     | All Users          |
| Status           | Completed          |

---

# Solution

## Step 1: Install python3-pip

First, I installed the `python3-pip` package on the Linux server.

I used:

```bash id="v0xj2k"
sudo dnf install python3-pip -y
```

`pip3` is the Python package manager used to install Python packages.

---

## Step 2: Verify pip3 Installation

After installing `python3-pip`, I verified that `pip3` was available:

```bash id="k8x0oz"
pip3 --version
```

This displayed the installed `pip3` version and confirmed that the package manager was successfully installed.

I also checked its location:

```bash id="w2e5qf"
which pip3
```

---

## Step 3: Install Ansible 4.7.0 Globally

Next, I installed the required Ansible version using `pip3`.

The required version was:

```text id="5h6y7x"
Ansible 4.7.0
```

I installed it globally using:

```bash id="1a8b3c"
sudo pip3 install ansible==4.7.0
```

Using `sudo` allowed the package to be installed at the system level instead of only for the current user.

This made Ansible available to other users on the server as well.

---

## Step 4: Verify Ansible Installation

After installing Ansible, I verified the installation using:

```bash id="4d5e6f"
ansible --version
```

The output displayed the installed Ansible version.

The important part was:

```text id="7g8h9i"
ansible [core ...]
```

Ansible 4.7.0 is an Ansible community package release that uses an Ansible Core dependency, so the `ansible --version` output reports the corresponding core version as well.

---

## Step 5: Verify Ansible is Globally Available

To confirm that Ansible was installed system-wide, I checked its location:

```bash id="0j1k2l"
which ansible
```

I could also verify the installation using:

```bash id="3m4n5o"
pip3 show ansible
```

The package information confirmed that Ansible was installed.

Because the installation was performed globally with `sudo pip3`, the Ansible command was available outside the original user's local environment.

---

# What I Learned

From this challenge, I learned and practiced:

* How to install `python3-pip` on a Linux server.
* How to verify the `pip3` installation.
* How to install a specific Python package version using `pip3`.
* How to install Ansible version `4.7.0`.
* How `sudo pip3` can be used for a system-wide installation.
* How to verify Ansible using `ansible --version`.
* How to find installed commands using `which`.
* How to check Python package information using `pip3 show`.
* The difference between installing a package for a single user and installing it globally.
* Why version-specific installations are important when a task requires an exact software version.

# Challenge Status

Day 08 — Completed Successfully 

**Ansible 4.7.0 installed globally and verified successfully.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
