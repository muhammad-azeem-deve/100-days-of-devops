# Day 07 - Commands

This file contains the commands used during **Day 07 of my 100 Days of DevOps Challenge**.

The objective was to configure **passwordless SSH authentication** for the user `thor` so that the user could access all required servers without using a password.

---

# Step 1: Access Server as thor

Connect to the server using the `thor` user:

```bash
ssh thor@<server-hostname>
```

---

# Step 2: Verify Current User

Check the current logged-in user:

```bash
whoami
```

Expected output:

```text
thor
```

---

# Step 3: Generate RSA SSH Key Pair

Generate an RSA SSH key pair:

```bash
ssh-keygen -t rsa
```

Press **Enter** to use the default key location.

The generated files are:

```text
~/.ssh/id_rsa
~/.ssh/id_rsa.pub
```

---

# Step 4: Verify SSH Keys

List the SSH directory:

```bash
ls -la ~/.ssh/
```

You should see files similar to:

```text
id_rsa
id_rsa.pub
```

---

# Step 5: View the Public Key

Display the public key:

```bash
cat ~/.ssh/id_rsa.pub
```

The public key can be copied to the remote servers.

---

# Step 6: Copy Public Key to Server 1

Use:


```bash
ssh-copy-id tony@stapp01
```

Enter the password when prompted for the initial key installation.

---

# Step 7: Copy Public Key to Server 2

Use:

```bash
ssh-copy-id steve@stapp02
```

---

# Step 8: Copy Public Key to Server 3


```bash
ssh-copy-id banner@stapp03
```

Repeat the command for every required server.

---

# Step 9: Test Passwordless SSH

After copying the public key, connect to Server 1:

```bash
ssh tony@stapp01
```

Then test Server 2:

```bash
ssh steve@satpp02
```

Then test Server 3:

```bash
ssh banner@stapp03
```

The SSH connection should be established without asking for the user's password.

---

# Step 10: Verify Authorized Keys

On a remote server, check the authorized keys file:

```bash
cat ~/.ssh/authorized_keys
```

The `thor` user's public key should be present.

---

# Step 11: Check SSH Directory Permissions

Check the `.ssh` directory:

```bash
ls -ld ~/.ssh
```

Check the authorized keys file:

```bash
ls -l ~/.ssh/authorized_keys
```

Typical secure permissions are:

```text
~/.ssh              → 700
authorized_keys     → 600
```

They can be set using:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

# Step 12: Troubleshoot SSH Authentication

If SSH still asks for a password, use verbose SSH output:

```bash
ssh -v thor@<server-hostname>
```

For more detailed debugging:

```bash
ssh -vv thor@<server-hostname>
```

Or:

```bash
ssh -vvv thor@<server-hostname>
```

These commands help identify authentication problems.

---

# Quick Command Summary

```bash
# Access server as thor
ssh thor@<server-hostname>

# Verify user
whoami

# Generate RSA key pair
ssh-keygen -t rsa

# Check generated keys
ls -la ~/.ssh/

# Display public key
cat ~/.ssh/id_rsa.pub

# Copy public key to Server 1
ssh-copy-id thor@<server1-hostname>

# Copy public key to Server 2
ssh-copy-id thor@<server2-hostname>

# Copy public key to Server 3
ssh-copy-id thor@<server3-hostname>

# Test passwordless SSH
ssh <user>@<server1-hostname>
ssh <user>@<server2-hostname>
ssh <user>@<server3-hostname>

# Verify authorized keys
cat ~/.ssh/authorized_keys

# Troubleshoot SSH
ssh -v thor@<server-hostname>
```

---

# SSH Key Files

The RSA key pair generated using `ssh-keygen -t rsa` consists of:

```text
Private Key:
~/.ssh/id_rsa

Public Key:
~/.ssh/id_rsa.pub
```

The private key should remain protected and should **never be copied to the remote servers**.

The public key is copied to:

```text
~/.ssh/authorized_keys
```

on the remote server.

---

# Important Lesson

For passwordless SSH authentication, the **public key is copied to the remote server**, while the **private key stays with the user generating the key pair**.

The main commands used in this challenge were:

```bash
ssh-keygen -t rsa
```

and:

```bash
ssh-copy-id thor@<server-hostname>
```

After the public key was installed on all required servers, `thor` could access them using:

```bash
ssh thor@<server-hostname>
```

without entering a password.
