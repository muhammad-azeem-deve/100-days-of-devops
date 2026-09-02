# Day 10 - Commands

This file contains the commands used during **Day 10 of my 100 Days of DevOps Challenge**.

The objective was to create a Bash script named `blog_archive.sh` on **App Server 3** that creates a ZIP archive of `/var/www/html/blog` and copies it to the Nautilus Storage Server without asking for a password.

---

# Step 1: Install ZIP Package

Install the required `zip` package manually:

```bash
sudo dnf install -y zip
```

The `zip` package must be installed before running the script.

---

# Step 2: Check Scripts Directory

Check the `/scripts` directory:

```bash
ls -la /scripts
```

---

# Step 3: Check Archives Directory

Check the archive directory:

```bash
ls -la /archives
```

---

# Step 4: Set Directory Ownership

Give the `banner` user ownership of the required directories:

```bash
sudo chown -R banner:banner /scripts /archives
```

---

# Step 5: Set Directory Permissions

Set the required permissions:

```bash
sudo chmod 755 /scripts /archives
```

Verify:

```bash
ls -la /scripts
```

```bash
ls -la /archives
```

---

# Step 6: Generate SSH Key

Generate an RSA SSH key pair:

```bash
ssh-keygen -t rsa
```

The generated keys are stored in:

```text
~/.ssh/id_rsa
~/.ssh/id_rsa.pub
```

---

# Step 7: Check SSH Directory

```bash
ls -a
```

Enter the SSH directory:

```bash
cd .ssh
```

Check the files:

```bash
ls
```

---

# Step 8: Display Public Key

Display the public key:

```bash
cat id_rsa.pub
```

The public key is used for passwordless SSH authentication.

---

# Step 9: Copy Public Key to Storage Server

Copy the public key to the Nautilus Storage Server:

```bash
ssh-copy-id natasha@ststor01
```

The first time, the remote user's password may be requested to install the public key.

---

# Step 10: Test Passwordless SSH

Connect to the storage server:

```bash
ssh natasha@ststor01
```

After the key has been configured, SSH should not ask for the password.

---

# Step 11: Create the Bash Script

Create the required script:

```bash
vi /scripts/blog_archive.sh
```

The script content:

```bash
#!/bin/bash

zip -r /archives/xfusioncorp_blog.zip /var/www/html/blog
scp /archives/xfusioncorp_blog.zip natasha@ststor01:/archives/
```

---

# Step 12: Check the Scripts Directory

```bash
ls -la /scripts
```

---

# Step 13: Make the Script Executable

Give execute permission to the script:

```bash
chmod +x /scripts/blog_archive.sh
```

Verify:

```bash
ls -l /scripts/blog_archive.sh
```

---

# Step 14: Execute the Script

Run the script:

```bash
/scripts/blog_archive.sh
```

The script creates:

```text
/archives/xfusioncorp_blog.zip
```

and then copies it to:

```text
natasha@ststor01:/archives/
```

---

# Step 15: Verify the Local Archive

Check the local archive directory:

```bash
ls -la /archives
```

Expected file:

```text
xfusioncorp_blog.zip
```

---

# Step 16: Connect to the Storage Server

```bash
ssh natasha@ststor01
```

---

# Step 17: Verify the Remote Archive

On the Nautilus Storage Server:

```bash
ls -la /archives
```

Expected file:

```text
xfusioncorp_blog.zip
```

---

# Step 18: Check Website Directory

To verify the source website content:

```bash
cd /var/www/html
```

List the website files:

```bash
ls
```

Check the blog directory:

```bash
ls blog
```

---

# Step 19: Inspect Website Files

The website files can be inspected using:

```bash
cat index.html
```

and:

```bash
ls blog
```

These checks help confirm that the source content exists before creating the archive.

---



# Complete Command Sequence

```bash
# Install zip
sudo dnf install -y zip

# Check directories
ls -la /scripts
ls -la /archives

# Set ownership
sudo chown -R banner:banner /scripts /archives

# Set permissions
sudo chmod 755 /scripts /archives

# Verify directories
ls -la /scripts
ls -la /archives

# Generate SSH key
ssh-keygen -t rsa

# Check SSH files
ls -a
cd .ssh
ls

# Display public key
cat id_rsa.pub

# Return to home directory
cd ..

# Copy public key to storage server
ssh-copy-id natasha@ststor01

# Test SSH connection
ssh natasha@ststor01

# Create script
vi /scripts/blog_archive.sh

# Make script executable
chmod +x /scripts/blog_archive.sh

# Verify script
ls -l /scripts/blog_archive.sh

# Run script
/scripts/blog_archive.sh

# Verify local archive
ls -la /archives

# Access storage server
ssh natasha@ststor01

# Verify remote archive
ls -la /archives
```

---

# Final Bash Script

The final `blog_archive.sh` script was:

```bash
#!/bin/bash

zip -r /archives/xfusioncorp_blog.zip /var/www/html/blog
scp /archives/xfusioncorp_blog.zip natasha@ststor01:/archives/
```

---

# Understanding the Script

## Shebang

```bash
#!/bin/bash
```

Specifies that the script should be executed using Bash.

---

## Create ZIP Archive

```bash
zip -r /archives/xfusioncorp_blog.zip /var/www/html/blog
```

Here:

```text
zip
```

creates a ZIP archive.

```text
-r
```

means recursive, so the contents of the `blog` directory and its subdirectories are included.

```text
/archives/xfusioncorp_blog.zip
```

is the destination archive.

```text
/var/www/html/blog
```

is the website directory being archived.

---

## Copy Archive to Storage Server

```bash
scp /archives/xfusioncorp_blog.zip natasha@ststor01:/archives/
```

This transfers the ZIP archive from App Server 3 to the Nautilus Storage Server.

The destination is:

```text
natasha@ststor01:/archives/
```

---

# Important Requirements

The script must satisfy the following requirements:

```text
Script Location:
 /scripts/blog_archive.sh

Source:
 /var/www/html/blog

Local Archive:
 /archives/xfusioncorp_blog.zip

Remote Server:
 natasha@ststor01

Remote Location:
 /archives/

Authentication:
 Passwordless SSH

Script:
 No sudo commands
```

---

# Important Lesson

This challenge showed how multiple Linux and DevOps concepts can be combined into one automation task.

The complete workflow was:

```text
Website Content
      ↓
/var/www/html/blog
      ↓
zip -r
      ↓
/archives/xfusioncorp_blog.zip
      ↓
scp
      ↓
Nautilus Storage Server
      ↓
/archives/xfusioncorp_blog.zip
```

The most important commands from this challenge were:

```bash
sudo dnf install -y zip
```

```bash
ssh-keygen -t rsa
```

```bash
ssh-copy-id natasha@ststor01
```

```bash
chmod +x /scripts/blog_archive.sh
```

and:

```bash
/scripts/blog_archive.sh
```

**Day 10 completed successfully.** 
