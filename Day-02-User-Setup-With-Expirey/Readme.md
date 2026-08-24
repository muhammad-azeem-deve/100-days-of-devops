# Day 02 - Create Linux User with Expiry Date

## Challenge Overview

This is Day 02 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Linux User Management ** and required creating a user on **Application Server 1** with a specific account expiration date.

### Challenge Requirement

Create a Linux user named `yousuf` on **Application Server 1** with an expiry date of **2027-04-15**.

The username must be in lowercase.

The objective was to access Application Server 1 using SSH, verify the server connection, create the user with the required expiry date, and verify both the user and its expiration date.

---

## Objectives

The objectives of this challenge are:

- Access Application Server 1 using SSH.
- Verify that the correct server has been accessed.
- Create a Linux user named `yousuf`.
- Set the user's account expiry date to `2027-04-15`.
- Verify that the user was created successfully.
- Verify the user's account expiration date.
- Understand the use of the `-e` option with `useradd`.
- Understand how to check account expiration using `chage`.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 02 |
| Server | Application Server 1 |
| User | `yousuf` |
| Username Format | Lowercase |
| Expiry Date | `2027-04-15` |
| Status | Completed |

---

# Solution

## Step 1: Access Application Server 1

First, I accessed **Application Server 1** using SSH.

The SSH command format is:

```bash
ssh tony@stapp01
```
## Step 2: Verify the Server Access

After connecting to the server, I verified that I was working on the correct server.

I used the following command:
```bash
hostname
```
## Step 3: Create the User with an Expiry Date

Next, I created the Linux user named yousuf and assigned the required expiry date.

The required expiry date was:
2027-04-15

I used the useradd command with the -e option:

```bash
sudo useradd -e 2027-04-15 yousuf
```

## Step 4: Verify the User

After creating the user, I verified that yousuf was successfully created.

I used the getent passwd command:

```bash
getent passwd yousuf | grep yousuf
```
Expected output:
```bash
yousuf:x:1001:1001::/home/yousuf:/bin/bash
```

## Step 5: Verify the User Expiration Date

After confirming that the user exists, I checked the account expiration information using the chage command.

I used:
```bash
sudo chage -l yousuf
```
The output should contain an expiration date similar to:

Account expires : Apr 15, 2027

# What I Learned

From this challenge, I learned and practiced:

- How SSH is used to access remote Linux servers.
- How to create Linux users with expiration date using -e flag.- How to assi.
- How to verify user with expiration date using sudo chage -l <username>
- How to verify Linux users using getent.
- How to inspect user information from /etc/passwd.
- How to use Linux command pipelines with |.

# Challenge Status

Day 02 — Completed Successfully 

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
