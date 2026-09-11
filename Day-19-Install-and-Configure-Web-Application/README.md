# Day 19 - Host Multiple Static Websites on Apache

## Challenge Overview

This is Day 19 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Apache Web Server Configuration and Hosting Multiple Static Websites**.

xFusionCorp Industries planned to host two static websites on **Application Server 2** in the Stratos Datacenter. The website development was still in progress, but the server needed to be prepared to host both websites.

The task required installing Apache, configuring it to listen on port `6200`, copying the website backups from the jump host, configuring both websites, and verifying them using `curl`.

### Challenge Requirement

The following requirements had to be completed on **Application Server 2**:

* Install the `httpd` package and its dependencies.
* Configure Apache to serve websites on port `6200`.
* Configure the `media` website to work at:
  `http://localhost:6200/media/`
* Configure the `cluster` website to work at:
  `http://localhost:6200/cluster/`
* Copy the website backups from the jump host.
* Place the websites in the Apache web root.
* Set the required permissions.
* Enable and start the Apache service.
* Verify both websites using `curl`.

---

## Objectives

The objectives of this challenge are:

* Access Application Server 2 using SSH.
* Install the Apache HTTP server.
* Verify that Apache was installed successfully.
* Change the Apache listening port from `80` to `6200`.
* Copy website backups from the jump host to Application Server 2.
* Configure the `media` website.
* Configure the `cluster` website.
* Set appropriate permissions for the website files.
* Enable and start the Apache service.
* Verify both websites using `curl`.
* Understand how Apache can host multiple websites using URL paths.

---

## Environment

| Item            | Details                                  |
| --------------- | ---------------------------------------- |
| Challenge       | 100 Days of DevOps                       |
| Platform        | KodeKloud                                |
| Day             | 19                                       |
| Server          | Application Server 2                     |
| Web Server      | Apache HTTPD                             |
| Package         | `httpd`                                  |
| Apache Port     | `6200`                                   |
| Website 1       | `media`                                  |
| Website 2       | `cluster`                                |
| Website Path 1  | `/media/`                                |
| Website Path 2  | `/cluster/`                              |
| Backup Location | `/home/thor/media`, `/home/thor/cluster` |
| Web Root        | `/var/www/html/`                         |
| Verification    | `curl`                                   |
| Status          | Completed                                |

---

# Solution

## Step 1: Access Application Server 2

First, I accessed **Application Server 2** using SSH.

The SSH command format is:

```bash
ssh steve@stapp02
```

---

## Step 2: Verify the Server

After connecting to the server, I verified that I was working on the correct server:

```bash
hostname
```

This confirmed that I was connected to Application Server 2 before making any changes.

---

## Step 3: Install Apache HTTPD

Next, I installed the Apache HTTP server using `dnf`:

```bash
sudo dnf install httpd -y
```

This installed the Apache package and its required dependencies.

---

## Step 4: Verify Apache Installation

After installation, I checked the Apache package:

```bash
httpd -v
```

This displayed the installed Apache version and confirmed that Apache was installed successfully.

I could also verify the package using:

```bash
rpm -q httpd
```

---

## Step 5: Change Apache Port

By default, Apache listens on port `80`.

The requirement was to configure Apache to listen on port:

```text
6200
```

I opened the Apache configuration file:

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

I located:

```text
Listen 80
```

and changed it to:

```text
Listen 6200
```

Then I saved the configuration file and exited.

---

## Step 6: Copy Website Backups from Jump Host

The website backups were available on the jump host:

```text
/home/thor/media
/home/thor/cluster
```

I copied the website directories from the jump host to the `/tmp` directory on Application Server 2.

For example:

```bash
scp -r /home/thor/media <username>@stapp02:/tmp/
```

and:

```bash
scp -r /home/thor/cluster <username>@stapp02:/tmp/
```

The files were first placed in `/tmp` on Application Server 2.

---

## Step 7: Move Websites to Apache Web Root

After copying the website directories to Application Server 2, I moved them into Apache's web root:

```bash
sudo mv /tmp/media /var/www/html/
```

```bash
sudo mv /tmp/cluster /var/www/html/
```

The final structure became:

```text
/var/www/html/
├── media/
└── cluster/
```

This allowed Apache to serve the websites using their respective URL paths.

---

## Step 8: Set Website Permissions

I made sure that Apache could access the website files.

I used:

```bash
sudo chmod -R 755 /var/www/html/media
```

and:

```bash
sudo chmod -R 755 /var/www/html/cluster
```

This provided the required read and directory traversal permissions for serving the static website files.

---

## Step 9: Enable Apache

I enabled Apache so that it would start automatically after a system reboot:

```bash
sudo systemctl enable httpd
```

Then I started the service:

```bash
sudo systemctl start httpd
```

---

## Step 10: Check Apache Status

I verified that Apache was running:

```bash
sudo systemctl status httpd
```

The service should show:

```text
active (running)
```

---

## Step 11: Verify Apache Port

I checked whether Apache was listening on port `6200`:

```bash
sudo ss -tulnp | grep 6200
```

Apache should be listening on:

```text
6200
```

---

## Step 12: Test the Media Website

I tested the first website using:

```bash
curl http://localhost:6200/media/
```

The website content was returned successfully.

This confirmed that the `media` website was accessible through:

```text
http://localhost:6200/media/
```

---

## Step 13: Test the Cluster Website

I tested the second website using:

```bash
curl http://localhost:6200/cluster/
```

The website content was returned successfully.

This confirmed that the `cluster` website was accessible through:

```text
http://localhost:6200/cluster/
```

---

# Final Apache Structure

After completing the configuration, the Apache web root contained:

```text
/var/www/html/
├── media/
│   └── website files
│
└── cluster/
    └── website files
```

Apache was configured to listen on:

```text
6200
```

Therefore, the websites were available at:

```text
http://localhost:6200/media/
```

and:

```text
http://localhost:6200/cluster/
```

---

# Verification

The final verification commands were:

```bash
sudo systemctl status httpd
```

```bash
sudo ss -tulnp | grep 6200
```

```bash
curl http://localhost:6200/media/
```

```bash
curl http://localhost:6200/cluster/
```

Both websites responded successfully.

---

# What I Learned

From this challenge, I learned and practiced:

* How to install Apache HTTPD using `dnf`.
* How to verify Apache installation.
* How Apache configuration files work.
* How to change the Apache listening port.
* How to configure Apache to listen on a custom port.
* How to copy files between Linux servers using `scp`.
* How to move website files into `/var/www/html/`.
* How Apache serves static website files.
* How multiple websites can be accessed through different URL paths.
* How to set permissions for website directories.
* How to enable and start the Apache service.
* How to check Apache service status.
* How to verify an open listening port using `ss`.
* How to test a web server from the command line using `curl`.

# Challenge Status

Day 19 — Completed Successfully 

**Two static websites hosted successfully on Apache using a custom port.**

`media` → `http://localhost:6200/media/`

`cluster` → `http://localhost:6200/cluster/`

100 Days. 100 Challenges. One DevOps Journey.

I will continue documenting each challenge.
