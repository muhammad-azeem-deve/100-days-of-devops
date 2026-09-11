# Day 19 - Commands

This file contains the commands used during **Day 19 of my 100 Days of DevOps Challenge**.

The objective was to install Apache on Application Server 2, configure it to use port `6200`, host the `media` and `cluster` websites, and verify them using `curl`.

---

# Step 1: Access Application Server 2

Connect to Application Server 2:

```bash
ssh <username>@<server2-hostname>
```

Example:

```bash
ssh tony@stapp02
```

---

# Step 2: Verify the Server

Check the hostname:

```bash
hostname
```

---

# Step 3: Install Apache

Install the Apache HTTP server:

```bash
sudo dnf install httpd -y
```

---

# Step 4: Verify Apache Installation

Check the Apache version:

```bash
httpd -v
```

Check the installed package:

```bash
rpm -q httpd
```

---

# Step 5: Open Apache Configuration

Open the Apache configuration file:

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Find:

```text
Listen 80
```

Change it to:

```text
Listen 6200
```

Save and exit the file.

---

# Step 6: Copy Media Website from Jump Host

Copy the `media` website backup to `/tmp` on Application Server 2:

```bash
scp -r /home/thor/media <username>@stapp02:/tmp/
```

---

# Step 7: Copy Cluster Website from Jump Host

Copy the `cluster` website backup to `/tmp`:

```bash
scp -r /home/thor/cluster <username>@stapp02:/tmp/
```

---

# Step 8: Move Media Website to Apache Web Root

Move the `media` directory:

```bash
sudo mv /tmp/media /var/www/html/
```

---

# Step 9: Move Cluster Website to Apache Web Root

Move the `cluster` directory:

```bash
sudo mv /tmp/cluster /var/www/html/
```

---

# Step 10: Check Website Directories

Verify that both directories exist:

```bash
ls -l /var/www/html/
```

Expected structure:

```text
media
cluster
```

---

# Step 11: Set Permissions for Media

Give the `media` directory appropriate permissions:

```bash
sudo chmod -R 755 /var/www/html/media
```

---

# Step 12: Set Permissions for Cluster

Give the `cluster` directory appropriate permissions:

```bash
sudo chmod -R 755 /var/www/html/cluster
```

---

# Step 13: Enable Apache

Enable Apache to start automatically after reboot:

```bash
sudo systemctl enable httpd
```

---

# Step 14: Start Apache

Start the Apache service:

```bash
sudo systemctl start httpd
```

---

# Step 15: Check Apache Status

Verify that Apache is running:

```bash
sudo systemctl status httpd
```

Expected status:

```text
active (running)
```

---

# Step 16: Verify Apache Port

Check whether Apache is listening on port `6200`:

```bash
sudo ss -tulnp | grep 6200
```

---

# Step 17: Test Media Website

Test the `media` website:

```bash
curl http://localhost:6200/media/
```

Expected result:

The HTML content of the `media` website should be returned.

---

# Step 18: Test Cluster Website

Test the `cluster` website:

```bash
curl http://localhost:6200/cluster/
```

Expected result:

The HTML content of the `cluster` website should be returned.

---

# Quick Command Summary

```bash
# Access Application Server 2
ssh <username>@<server2-hostname>

# Verify server
hostname

# Install Apache
sudo dnf install httpd -y

# Verify Apache
httpd -v

# Open Apache configuration
sudo nano /etc/httpd/conf/httpd.conf

# Change Apache port
Listen 80
# To:
Listen 6200

# Copy websites from jump host
scp -r /home/thor/media <username>@stapp02:/tmp/
scp -r /home/thor/cluster <username>@stapp02:/tmp/

# Move websites to Apache web root
sudo mv /tmp/media /var/www/html/
sudo mv /tmp/cluster /var/www/html/

# Set permissions
sudo chmod -R 755 /var/www/html/media
sudo chmod -R 755 /var/www/html/cluster

# Enable Apache
sudo systemctl enable httpd

# Start Apache
sudo systemctl start httpd

# Check Apache status
sudo systemctl status httpd

# Verify port
sudo ss -tulnp | grep 6200

# Test Media website
curl http://localhost:6200/media/

# Test Cluster website
curl http://localhost:6200/cluster/
```

---

# Final Verification

Verify Apache:

```bash
sudo systemctl status httpd
```

Verify port `6200`:

```bash
sudo ss -tulnp | grep 6200
```

Verify Media:

```bash
curl http://localhost:6200/media/
```

Verify Cluster:

```bash
curl http://localhost:6200/cluster/
```

---

# Website URLs

After successful configuration:

```text
Media Website:
http://localhost:6200/media/
```

```text
Cluster Website:
http://localhost:6200/cluster/
```

---

# Important Lesson

When hosting multiple static websites using Apache, the website directories can be placed inside the Apache document root:

```text
/var/www/html/
```

For this challenge:

```text
/var/www/html/media/
```

was accessed through:

```text
http://localhost:6200/media/
```

and:

```text
/var/www/html/cluster/
```

was accessed through:

```text
http://localhost:6200/cluster/
```

The most important Apache configuration change was:

```text
Listen 6200
```

After changing the port and placing the website files correctly, Apache was started and both websites were successfully tested using `curl`.

**Day 19 completed successfully.** 🚀
