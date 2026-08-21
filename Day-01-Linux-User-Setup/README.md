# Day 01: Linux User Setup with Non-Interactive Shell

##  100 Days of DevOps Challenge — KodeKloud

This is **Day 01** of my **100 Days of DevOps Challenge** on KodeKloud.

In this challenge, I worked with Linux user management and learned how to create a user with a **non-interactive shell** on a remote Linux server.

---

## Challenge Objective

The task was to:

- Access **Server 1** using SSH.
- Create a Linux user named `kareem`.
- Assign `/sbin/nologin` as the user's shell.
- Verify that the user was created successfully.
- Verify that the user has a non-interactive shell.

---

## Lab Environment

| Configuration | Details |
|---|---|
| Platform | KodeKloud |
| Challenge | 100 Days of DevOps |
| Day | 01 |
| Server | Server 1 |
| Hostname | `stapp01` |
| Username | `kareem` |
| Shell | `/sbin/nologin` |
| Access Method | SSH |

---

# Implementation

## Step 1 — Access Server 1

First, I connected to Server 1 using SSH.

```bash
ssh tony@stapp01
```
## Step 2 — Create the Linux User

I created the user kareem using the useradd command.
```bash
sudo useradd -s /sbin/nologin kareem
```
## Step 3 — Verify the User

After creating the user, I verified that the user exists on the system.

```bash
getent passwd kareem
```
Expected output:
```bash
kareem:x:<UID>:<GID>::/home/kareem:/sbin/nologin
```
## Step 4 — Verify Using /etc/passwd

I also verified the user directly from the /etc/passwd file.

```bash
grep '^kareem:' /etc/passwd
```
Expected output:
```bash
kareem:x:<UID>:<GID>::/home/kareem:/sbin/nologin
```
# What I Learned

From this challenge, I learned and practiced:

- How SSH is used to access remote Linux servers.
- How to create Linux users using useradd.
- How to assign a specific shell to a user.
- What a non-interactive shell means.
- How /sbin/nologin works.
- How to verify Linux users using getent.
- How to inspect user information from /etc/passwd.
- How to use Linux command pipelines with |.
- How to extract specific fields using cut.

# Challenge Status

## Day 01 — Completed 

This is the first step in my 100 Days of DevOps journey.

I will continue documenting each challenge.
