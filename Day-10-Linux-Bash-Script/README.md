# Day 10 - Bash Script to Archive Website Content

## Challenge Overview

This is Day 10 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Bash Scripting, File Archiving, SSH Authentication, and Secure File Transfer**.

The production support team of **xFusionCorp Industries** needed a Bash script to automate the process of archiving website content. The static website was running on **App Server 3** in the Stratos Datacenter.

The task was to create a Bash script named `blog_archive.sh` under the `/scripts` directory. The script had to create a ZIP archive of the website's blog directory, store it locally, and then copy the archive to the **Nautilus Storage Server** without asking for a password.

---

## Challenge Requirement

The task required creating:

```text
/scripts/blog_archive.sh
```

The script needed to:

1. Create a ZIP archive named `xfusioncorp_blog.zip`.
2. Archive the `/var/www/html/blog` directory.
3. Store the archive in `/archives/` on App Server 3.
4. Copy the archive to the Nautilus Storage Server.
5. Store the copied archive in `/archives/` on the storage server.
6. Configure passwordless SSH authentication so the script does not ask for a password.
7. Make sure the respective server user can execute the script.
8. Do not use `sudo` inside the script.
9. Install the `zip` package manually outside the script.

---

## Objectives

The objectives of this challenge are:

* Access App Server 3 using SSH.
* Install the `zip` package manually.
* Verify the `/scripts` and `/archives` directories.
* Configure appropriate ownership and permissions.
* Generate an SSH key for passwordless authentication.
* Copy the public SSH key to the Nautilus Storage Server.
* Test passwordless SSH access.
* Create the `blog_archive.sh` Bash script.
* Create a ZIP archive of the website blog directory.
* Store the archive in `/archives/`.
* Copy the archive to the Nautilus Storage Server.
* Make the script executable.
* Execute and test the script.
* Verify the archive on the storage server.

---

## Environment

| Item               | Details                          |
| ------------------ | -------------------------------- |
| Challenge          | 100 Days of DevOps               |
| Platform           | KodeKloud                        |
| Day                | 10                               |
| Application Server | App Server 3                     |
| Application User   | `banner`                         |
| Website Directory  | `/var/www/html/blog`             |
| Script             | `/scripts/blog_archive.sh`       |
| Local Archive      | `/archives/xfusioncorp_blog.zip` |
| Storage Server     | Nautilus Storage Server          |
| Storage User       | `natasha`                        |
| Remote Archive     | `/archives/xfusioncorp_blog.zip` |
| Archive Format     | ZIP                              |
| Authentication     | Passwordless SSH                 |
| Status             | Completed                        |

---

# Solution

## Step 1: Access App Server 3

First, I accessed **App Server 3** using SSH.

The SSH command format is:

```bash
ssh <username>@<server3-hostname>
```

After connecting, I verified the server:

```bash
hostname
```

This ensured that I was working on the correct application server.

---

## Step 2: Install the ZIP Package

The challenge specifically required the `zip` package to be installed manually before executing the script.

I installed it using:

```bash
sudo dnf install -y zip
```

The `zip` package is required to create the ZIP archive of the website files.

The package installation was performed **outside the script**, as required by the challenge.

---

## Step 3: Check the Required Directories

I checked the `/scripts` directory:

```bash
ls -la /scripts
```

I also checked the archive directory:

```bash
ls -la /archives
```

The `/scripts` directory was required to store the Bash script, while `/archives` was required for temporary local archive storage.

---

## Step 4: Configure Directory Ownership and Permissions

I made sure that the required user had access to the `/scripts` and `/archives` directories.

I used:

```bash
sudo chown -R banner:banner /scripts /archives
```

Then I set the required directory permissions:

```bash
sudo chmod 755 /scripts /archives
```

Finally, I verified the permissions:

```bash
ls -la /scripts
```

and:

```bash
ls -la /archives
```

This ensured that the `banner` user could work with the required directories.

---

## Step 5: Generate an SSH Key

The script needed to copy the archive to the Nautilus Storage Server without asking for a password.

For this purpose, I generated an RSA SSH key pair:

```bash
ssh-keygen -t rsa
```

The generated keys were stored inside:

```text
~/.ssh/
```

The important files were:

```text
id_rsa
id_rsa.pub
```

The private key remained on App Server 3, while the public key was copied to the storage server.

---

## Step 6: Verify the SSH Key

I checked the `.ssh` directory:

```bash
ls -a
```

Then:

```bash
cd .ssh
```

and:

```bash
ls
```

I checked the public key:

```bash
cat id_rsa.pub
```

The public key was then used for passwordless SSH authentication.

---

## Step 7: Configure Passwordless SSH

I copied the public key to the Nautilus Storage Server using:

```bash
ssh-copy-id natasha@ststor01
```

After copying the key, I tested the connection:

```bash
ssh natasha@ststor01
```

The connection worked without requiring the password after the key was properly configured.

This was important because the Bash script must be able to copy the archive automatically without waiting for a password.

---

## Step 8: Create the Bash Script

I created the required script:

```text
/scripts/blog_archive.sh
```

I opened the file using:

```bash
vi /scripts/blog_archive.sh
```

The script performs two main operations:

1. Creates the ZIP archive.
2. Copies the archive to the Nautilus Storage Server.

The script follows the required workflow:

```bash
#!/bin/bash

zip -r /archives/xfusioncorp_blog.zip /var/www/html/blog
scp /archives/xfusioncorp_blog.zip natasha@ststor01:/archives/
```

The script does **not** use `sudo`, as required by the challenge.

---

## Step 9: Make the Script Executable

After creating the script, I gave it execute permission:

```bash
chmod +x /scripts/blog_archive.sh
```

I verified the script:

```bash
ls -l /scripts
```

The script should have execute permission for the appropriate users.

---

## Step 10: Execute the Script

I executed the script using its full path:

```bash
/scripts/blog_archive.sh
```

The script then:

```text
/var/www/html/blog
        ↓
   ZIP Archive
        ↓
/archives/xfusioncorp_blog.zip
        ↓
   SCP Transfer
        ↓
Nautilus Storage Server
        ↓
/archives/xfusioncorp_blog.zip
```

---

## Step 11: Verify the Archive

After executing the script, I verified the local archive:

```bash
ls -la /archives
```

I then connected to the Nautilus Storage Server:

```bash
ssh natasha@ststor01
```

and checked the remote archive directory:

```bash
ls -la /archives
```

The expected file was:

```text
xfusioncorp_blog.zip
```

This confirmed that the archive had been successfully created and copied to the storage server.

---

## Step 12: Verify Website Files

During testing, I also inspected the website files to make sure the source directory contained the expected content.

I accessed:

```bash
cd /var/www/html
```

Then checked:

```bash
ls
```

and:

```bash
ls blog
```

This confirmed that the `blog` directory existed and contained website content that could be archived.

---

# Troubleshooting

During the challenge, I performed several checks while developing and testing the script.

I checked whether the `/scripts` directory existed:

```bash
ls -la /scripts
```

I also checked the archive directory:

```bash
ls -la /archives
```

I verified the SSH configuration and generated an RSA key so that the `scp` command could transfer the archive without requesting a password.

I tested the connection using:

```bash
ssh natasha@ststor01
```

I also executed the script multiple times while checking and modifying it:

```bash
/scripts/blog_archive.sh
```

When required, I reopened the script:

```bash
vi /scripts/blog_archive.sh
```

and tested it again.

This helped me verify that the archive was being created correctly and transferred to the storage server.

---

# Bash Script

The final script structure was:

```bash
#!/bin/bash

zip -r /archives/xfusioncorp_blog.zip /var/www/html/blog
scp /archives/xfusioncorp_blog.zip natasha@ststor01:/archives/
```

The script does not contain `sudo`, satisfying the challenge requirement.

---

# What I Learned

From this challenge, I learned and practiced:

* How to install packages using `dnf`.
* How to create Bash scripts.
* How to use the `zip` command to archive directories.
* How to use `scp` to transfer files between Linux servers.
* How SSH public-key authentication works.
* How to configure passwordless SSH using `ssh-copy-id`.
* How to generate RSA SSH keys using `ssh-keygen`.
* How to modify file and directory ownership using `chown`.
* How to modify permissions using `chmod`.
* How to make a Bash script executable.
* How to automate repetitive system administration tasks.
* How to verify files on both the source and destination servers.
* Why automation scripts should avoid interactive password prompts.
* The importance of testing a script after creating it.

# Challenge Status

Day 10 — Completed Successfully 

**Created a Bash automation script to archive website content and transfer the backup to a remote storage server using passwordless SSH.**

This challenge combined **Bash scripting + Linux permissions + ZIP archiving + SSH authentication + SCP file transfer** into one practical DevOps task.

100 Days. 100 Challenges. One DevOps Journey.

I will continue documenting each challenge.
