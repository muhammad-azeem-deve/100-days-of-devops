# Day 07 - Linux SSH Authentication

## Challenge Overview

This is Day 07 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Linux SSH Authentication** and required configuring passwordless SSH authentication for the user `thor`.

The objective was to generate an SSH key pair on the server where the `thor` user was working, copy the public key to all required servers, and then access those servers using SSH without entering a password.

### Challenge Requirement

Configure **passwordless SSH authentication** for the user `thor` so that `thor` can access all required servers without using a password.

The process involved:

1. Generate an SSH key pair for the `thor` user.
2. Use the generated public key for authentication.
3. Copy the public key to all required servers.
4. Verify that the public key was installed correctly.
5. Connect to all servers without entering a password.

---

## Objectives

The objectives of this challenge are:

* Access the server using the `thor` user.
* Generate an SSH RSA key pair.
* Understand the difference between a private key and a public key.
* Copy the public SSH key to remote servers.
* Configure passwordless SSH authentication.
* Access all required servers without entering a password.
* Verify that passwordless authentication is working correctly.
* Understand how SSH public-key authentication works.

---

## Environment

| Item           | Details              |
| -------------- | -------------------- |
| Challenge      | 100 Days of DevOps   |
| Platform       | KodeKloud            |
| Day            | 07                   |
| User           | `thor`               |
| Authentication | SSH Public Key       |
| Key Type       | RSA                  |
| Authentication | Passwordless         |
| Servers        | All Required Servers |
| Status         | Completed            |

---

# Solution

## Step 1: Access the Server as thor

First, I accessed the server where the `thor` user was available.

The SSH command format was:

```bash
ssh thor@<server-hostname>
```

After logging in, I verified the current user:

```bash
whoami
```

Expected output:

```text
thor
```

---

## Step 2: Generate an SSH Key Pair

Next, I generated an RSA SSH key pair for the `thor` user.

I used:

```bash
ssh-keygen -t rsa
```

During the key generation process, SSH asked for a location to save the key and a passphrase.

I used the default location and continued through the prompts.

The key pair consists of two files:

```text
~/.ssh/id_rsa
~/.ssh/id_rsa.pub
```

The private key is:

```text
~/.ssh/id_rsa
```

The public key is:

```text
~/.ssh/id_rsa.pub
```

---

## Step 3: Understand the SSH Key Pair

The SSH key pair contains:

### Private Key

```text
~/.ssh/id_rsa
```

The private key must be kept secure and should **never be shared** with other users or servers.

### Public Key

```text
~/.ssh/id_rsa.pub
```

The public key can be copied to remote servers.

The remote server uses the public key to authenticate the user when the corresponding private key is presented.

---

## Step 4: Copy the Public Key to the Servers

After generating the SSH key pair, I copied the public key to each required server using `ssh-copy-id`.

The command format was:

```bash
ssh-copy-id <user>@<server-hostname>
```

For example:

```bash
ssh-copy-id tony@stapp01
```

I repeated the same process for all required servers.

The public key is added to the remote user's:

```text
~/.ssh/authorized_keys
```

file.

---

## Step 5: Authenticate with the Remote Server

After copying the public key, I connected to the remote server using:

```bash
ssh <user>@<server-hostname>
```

The SSH client used the private key from the local machine to authenticate against the public key stored on the remote server.

After successful configuration, I was able to access the server **without entering the user's password**.

---

## Step 6: Verify Passwordless SSH

I tested SSH access to each required server:

```bash
ssh tony@stapp01
```

```bash
ssh steve@stapp02
```

```bash
ssh banner@stapp03
```

The connection was established without asking for the user's password.

This confirmed that passwordless SSH authentication was configured successfully.

---

## Step 7: Verify the Public Key on the Remote Server

After accessing a remote server, I could verify that the public key had been added using:

```bash
cat ~/.ssh/authorized_keys
```

The public key generated on the `thor` server should be present in this file.

---

# How Passwordless SSH Works

The authentication process works using a public/private key pair.

```text
                 THOR SERVER
                     |
                     |
              Private Key
             ~/.ssh/id_rsa
                     |
                     |
                     v
              SSH Authentication
                     |
                     |
                     v
              REMOTE SERVER
                     |
                     |
          ~/.ssh/authorized_keys
                     |
                     |
              Public Key
             id_rsa.pub
```

The private key remains on the client machine, while the public key is installed on the remote server.

When `thor` connects using SSH, the SSH server verifies the authentication using the matching public key.

---

# Troubleshooting

While configuring passwordless SSH, it is important to make sure that:

* The SSH key is generated for the correct user.
* The public key is copied to the correct remote user.
* The public key exists in `~/.ssh/authorized_keys`.
* The private key remains on the client/server where it was generated.
* SSH permissions are configured correctly.
* The correct hostname is used when connecting to each server.

If SSH still asks for a password, the connection can be tested in verbose mode:

```bash
ssh -v thor@<server-hostname>
```

This provides detailed information about the SSH authentication process.

---

# What I Learned

From this challenge, I learned and practiced:

* How SSH public-key authentication works.
* How to generate an RSA SSH key pair.
* The difference between public and private SSH keys.
* How to use `ssh-keygen`.
* How to use `ssh-copy-id`.
* How SSH stores authorized public keys in `authorized_keys`.
* How to configure passwordless SSH authentication.
* How to connect to remote Linux servers without entering a password.
* Why private SSH keys must be protected.
* How to troubleshoot SSH authentication using verbose mode.
* How passwordless authentication can make server administration easier and more secure.

# Challenge Status

Day 07 — Completed Successfully 

**Passwordless SSH configured successfully for `thor` across all required servers.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
