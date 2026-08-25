# Day 03 - Secure Root SSH Access

## Challenge Overview

This is **Day 03** of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Linux SSH Security**. The objective was to secure the SSH configuration by disabling direct root login on the required servers.

This challenge helped me understand how SSH configuration works, how to modify `sshd_config`, how to validate SSH configuration, and how to restart the SSH service correctly.

---

## Challenge Requirement

The task was to **disable direct root SSH login** on the required servers.

The SSH configuration parameter:

```text
PermitRootLogin yes
```

needed to be changed to:

```text
PermitRootLogin no
```

This prevents users from directly logging into the server through SSH as the `root` user.

---

## Servers Used

The task involved working with the following servers:

* Server 1
* Server 2
* Server 3

The configuration needed to be applied correctly on each required server.

---

## Implementation Steps

### 1. SSH into Server 1

First, I connected to **Server 1** using SSH.

```bash
ssh <username>@<server1-hostname>
```

---

### 2. Edit SSH Configuration

I opened the SSH daemon configuration file:

```bash
sudo vi /etc/ssh/sshd_config
```

I located:

```text
PermitRootLogin yes
```

and changed it to:

```text
PermitRootLogin no
```

Then I saved the file and exited the editor.

---

### 3. Restart the SSH Service

After modifying the configuration, I restarted the SSH service:

```bash
sudo systemctl restart sshd
```

This step is important because changing `sshd_config` does not automatically reload the running SSH daemon.

---

### 4. Repeat the Configuration on Other Servers

I followed the same process on **Server 2** and **Server 3**.

For each server:

1. SSH into the server.
2. Open `/etc/ssh/sshd_config`.
3. Change `PermitRootLogin yes` to `PermitRootLogin no`.
4. Save the configuration.
5. Restart the SSH service.

---

## Configuration Verification

After making the changes, I verified the SSH configuration using:

```bash
sudo sshd -t
```

If the command returns no output, the SSH configuration syntax is valid.

I also verified the configuration by checking the relevant setting:

```bash
sudo grep "^PermitRootLogin" /etc/ssh/sshd_config
```

Expected output:

```text
PermitRootLogin no
```

---

## Troubleshooting

During the challenge, I initially used the wrong approach and faced several problems.

### Problem 1 - SSH Service Restart Commands Were Not Working

I initially tried commands such as:

```bash
service ssh restart
```

and:

```bash
systemctl restart sshd
```

but received errors such as:

```text
service: not found
```

or:

```text
systemctl: not found
```

I also tried:

```bash
killall
```

which was not available.

I then attempted to start the SSH daemon directly:

```bash
sudo /usr/sbin/sshd
```

Although this helped me understand how the SSH daemon works, it was **not the correct approach for completing the KodeKloud task**.

---

### Problem 2 - Configuration Was Not Updated on the Correct Server

When I submitted the task, the `sshd_config` file was reported as not correctly updated.

After troubleshooting, I realized that I had been performing the configuration steps incorrectly and was not consistently applying the changes on the required servers.

---

### Problem 3 - Repeated Attempts

I had to redo the task around **3–4 times** because of issues while updating the `sshd_config` file on the different servers.

Eventually, I followed the correct server-by-server procedure and completed the challenge successfully.

---

## What I Learned

Through this challenge, I learned:

* How SSH daemon configuration works.
* The purpose of `/etc/ssh/sshd_config`.
* How to disable direct root SSH login.
* The difference between changing a configuration file and applying the configuration.
* How to validate SSH configuration using `sshd -t`.
* How to restart the SSH daemon correctly.
* The importance of applying configuration changes to the **correct server**.
* The importance of carefully verifying changes before submitting a DevOps task.
* Troubleshooting SSH-related problems in a Linux environment.

---

## Final Configuration

The final SSH configuration should contain:

```text
PermitRootLogin no
```

And the configuration should pass:

```bash
sudo sshd -t
```

without displaying an error.

---

## Challenge Status

**Status:** Completed Successfully 

**Main Topic:** Linux SSH Security

**Key Configuration:** `PermitRootLogin no`

**Configuration File:** `/etc/ssh/sshd_config`

---

## Conclusion

Day 03 was a good hands-on experience with Linux SSH security. The biggest lesson from this challenge was not only how to disable root SSH access, but also how important it is to make changes on the **correct server**, validate the configuration, and properly restart the required service.

After several attempts and troubleshooting, I successfully completed the challenge.
