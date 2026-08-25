# Day 03 - Commands

This file contains the commands used during **Day 03 of the 100 Days of DevOps Challenge**.

The objective was to disable direct root SSH login by changing `PermitRootLogin` from `yes` to `no`.

---

## 1. Connect to the Server

SSH into the required server:

```bash
ssh <username>@<server-hostname>
```

Example:

```bash
ssh user@server1
```

---

## 2. Open SSH Configuration

Open the SSH daemon configuration file:

```bash
sudo vi /etc/ssh/sshd_config
```

You can also use:

```bash
sudo nano /etc/ssh/sshd_config
```

---

## 3. Change Root Login Setting

Find:

```text
PermitRootLogin yes
```

Change it to:

```text
PermitRootLogin no
```

Save the file and exit.

---

## 4. Verify the Configuration

Test the SSH configuration syntax:

```bash
sudo sshd -t
```

If there is no output, the configuration syntax is valid.

---

## 5. Check the Root Login Setting

Use:

```bash
sudo grep "^PermitRootLogin" /etc/ssh/sshd_config
```

Expected result:

```text
PermitRootLogin no
```

---

## 6. Restart SSH Service

Restart the SSH daemon:

```bash
sudo systemctl restart sshd
```

On systems where the service is named `ssh`, the command may be:

```bash
sudo systemctl restart ssh
```

For the KodeKloud environment used in this challenge, the required command was:

```bash
sudo systemctl restart sshd
```

---

## 7. Verify SSH Service

Check the SSH service status:

```bash
sudo systemctl status sshd
```

If the service is named `ssh`:

```bash
sudo systemctl status ssh
```

---

## 8. Check SSH Configuration Again

Run:

```bash
sudo sshd -t
```

A successful validation normally produces no output.

---

## 9. Verify SSH Configuration

Check the final configuration:

```bash
sudo grep "PermitRootLogin" /etc/ssh/sshd_config
```

Expected configuration:

```text
PermitRootLogin no
```

---

## 10. Repeat on Other Servers

Perform the same process on each required server.

### Server 1

```bash
ssh tony@stapp01
sudo vi /etc/ssh/sshd_config
sudo systemctl restart sshd
sudo sshd -t
sudo grep "^PermitRootLogin" /etc/ssh/sshd_config
```

### Server 2

```bash
ssh steve@stapp02
sudo vi /etc/ssh/sshd_config
sudo systemctl restart sshd
sudo sshd -t
sudo grep "^PermitRootLogin" /etc/ssh/sshd_config
```

### Server 3

```bash
ssh banner@stapp03
sudo vi /etc/ssh/sshd_config
sudo systemctl restart sshd
sudo sshd -t
sudo grep "^PermitRootLogin" /etc/ssh/sshd_config
```

---

## 11. Troubleshooting Commands I Tried

During the challenge, I initially tried:

```bash
service ssh restart
```

```bash
systemctl restart sshd
```

```bash
killall
```

Some of these commands were unavailable in the environment and returned errors such as:

```text
service: not found
```

```text
systemctl: not found
```

I also tried starting the SSH daemon directly:

```bash
sudo /usr/sbin/sshd
```

However, this was not the correct approach for completing the challenge.

The correct approach was to modify the configuration on the required server and restart the SSH daemon using:

```bash
sudo systemctl restart sshd
```

---

## 12. Final Verification

The most important final checks were:

```bash
sudo sshd -t
```

and:

```bash
sudo grep "^PermitRootLogin" /etc/ssh/sshd_config
```

Expected result:

```text
PermitRootLogin no
```

---

## Quick Command Summary

```bash
# Connect to server
ssh <username>@<server-hostname>

# Open SSH configuration
sudo vi /etc/ssh/sshd_config

# Change:
PermitRootLogin yes

# To:
PermitRootLogin no

# Validate configuration
sudo sshd -t

# Restart SSH
sudo systemctl restart sshd

# Verify configuration
sudo grep "^PermitRootLogin" /etc/ssh/sshd_config

# Check service
sudo systemctl status sshd
```

---

## Important Lesson

Always make sure you are connected to the **correct server before modifying `/etc/ssh/sshd_config`**.

For this challenge, the configuration had to be correctly applied to the required servers. A change made on the wrong server will not satisfy the task even if the configuration itself is correct.
