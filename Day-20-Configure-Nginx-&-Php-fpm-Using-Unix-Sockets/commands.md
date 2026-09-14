# Day 20 - Commands

This file contains the commands used during **Day 20 of my 100 Days of DevOps Challenge**.

The objective was to install and configure **Nginx and PHP-FPM 8.2** on Application Server 1 and make the PHP application accessible through port `8095`.

---

# Step 1: Access Application Server 1

Connect to Application Server 1:

```bash
ssh <username>@stapp01
```

Verify the server:

```bash
hostname
```

---

# Step 2: Install Nginx

Install Nginx:

```bash
sudo yum install nginx -y
```

---

# Step 3: Configure Nginx

Open the Nginx configuration file:

```bash
sudo vi /etc/nginx/nginx.conf
```

Configure Nginx to:

```text
Listen on port: 8095
Document Root: /var/www/html
PHP-FPM Socket: /var/run/php-fpm/default.sock
```

---

# Step 4: Restart Nginx

```bash
sudo systemctl restart nginx
```

---

# Step 5: Enable Nginx

```bash
sudo systemctl enable nginx
```

---

# Step 6: Check Nginx Status

```bash
sudo systemctl status nginx
```

---

# Step 7: Install EPEL Repository

Install EPEL:

```bash
sudo dnf install -y epel-release
```

---

# Step 8: Install Remi Repository

Install the Remi repository:

```bash
sudo dnf install -y https://rpms.remirepo.net/enterprise/remi-release-9.rpm
```

---

# Step 9: Reset PHP Module

Reset the existing PHP module:

```bash
sudo dnf module reset php -y
```

---

# Step 10: Enable PHP 8.2

Enable the Remi PHP 8.2 module:

```bash
sudo dnf module enable php:remi-8.2 -y
```

---

# Step 11: Install PHP 8.2 and PHP-FPM

Install PHP, PHP-FPM, and required extensions:

```bash
sudo dnf install -y php php-fpm php-cli php-common php-mysqlnd php-gd php-mbstring php-xml php-zip php-json php-opcache
```

---

# Step 12: Verify PHP Version

```bash
php -v
```

The required version was:

```text
PHP 8.2.x
```

---

# Step 13: Verify PHP-FPM Version

```bash
php-fpm -v
```

This confirms the installed PHP-FPM version.

---

# Step 14: Create PHP-FPM Directory

Create the required parent directory:

```bash
sudo mkdir -p /var/run/php-fpm
```

---

# Step 15: Configure PHP-FPM

Open the PHP-FPM pool configuration:

```bash
sudo vi /etc/php-fpm.d/www.conf
```

Configure the PHP-FPM socket:

```text
listen = /var/run/php-fpm/default.sock
```

Save and exit.

---

# Step 16: Configure Nginx for PHP-FPM

Open the Nginx configuration:

```bash
sudo vi /etc/nginx/nginx.conf
```

Configure the PHP location to use:

```text
/var/run/php-fpm/default.sock
```

The PHP requests should be passed to PHP-FPM through this Unix socket.

---

# Step 17: Restart PHP-FPM

```bash
sudo systemctl restart php-fpm
```

---

# Step 18: Restart Nginx

```bash
sudo systemctl restart nginx
```

---

# Step 19: Verify PHP Version

```bash
php -v
```

---

# Step 20: Verify PHP-FPM Version

```bash
php-fpm -v
```

---

# Step 21: Check PHP-FPM Service

```bash
sudo systemctl status php-fpm
```

The service should be running successfully.

---

# Step 22: Check Nginx Service

```bash
sudo systemctl status nginx --no-pager
```

---

# Step 23: Verify PHP-FPM Socket

Check that the required Unix socket exists:

```bash
ls -l /var/run/php-fpm/default.sock
```

Expected socket location:

```text
/var/run/php-fpm/default.sock
```

---

# Step 24: Adjust Socket Ownership

During troubleshooting, I checked and adjusted the socket ownership:

```bash
sudo chown -R nginx:nginx /var/run/php-fpm/default.sock
```

Then verified it:

```bash
ls -l /var/run/php-fpm/default.sock
```

---

# Step 25: Test Nginx Configuration

Before testing the application, validate the Nginx configuration:

```bash
sudo nginx -t
```

A successful configuration test should report that the syntax is OK and the configuration test is successful.

---

# Step 26: Test the PHP Application

From the **jump host**, run:

```bash
curl http://stapp01:8095/index.php
```

If the PHP application responds correctly, the Nginx and PHP-FPM configuration is working.

---

# Quick Command Summary

```bash
# Access Application Server 1
ssh <username>@stapp01

# Verify server
hostname

# Install Nginx
sudo yum install nginx -y

# Configure Nginx
sudo vi /etc/nginx/nginx.conf

# Restart and enable Nginx
sudo systemctl restart nginx
sudo systemctl enable nginx

# Check Nginx
sudo systemctl status nginx

# Install EPEL
sudo dnf install -y epel-release

# Install Remi repository
sudo dnf install -y https://rpms.remirepo.net/enterprise/remi-release-9.rpm

# Reset PHP module
sudo dnf module reset php -y

# Enable PHP 8.2
sudo dnf module enable php:remi-8.2 -y

# Install PHP 8.2 and PHP-FPM
sudo dnf install -y php php-fpm php-cli php-common php-mysqlnd php-gd php-mbstring php-xml php-zip php-json php-opcache

# Verify PHP
php -v

# Verify PHP-FPM
php-fpm -v

# Create PHP-FPM directory
sudo mkdir -p /var/run/php-fpm

# Configure PHP-FPM
sudo vi /etc/php-fpm.d/www.conf

# Configure Nginx
sudo vi /etc/nginx/nginx.conf

# Restart PHP-FPM
sudo systemctl restart php-fpm

# Restart Nginx
sudo systemctl restart nginx

# Check PHP-FPM
sudo systemctl status php-fpm

# Check Nginx
sudo systemctl status nginx --no-pager

# Check PHP-FPM socket
ls -l /var/run/php-fpm/default.sock

# Adjust socket ownership
sudo chown -R nginx:nginx /var/run/php-fpm/default.sock

# Verify socket
ls -l /var/run/php-fpm/default.sock

# Test Nginx configuration
sudo nginx -t

# Test application from jump host
curl http://stapp01:8095/index.php
```

---

# Important Configuration Values

## Nginx

```text
Port:
8095

Document Root:
/var/www/html
```

---

## PHP-FPM

```text
PHP Version:
8.2

Unix Socket:
/var/run/php-fpm/default.sock
```

---

## Application Files

The challenge provided these files:

```text
/var/www/html/index.php
/var/www/html/info.php
```

**Important:** These files were provided as part of the task and were **not modified**.

---

# Troubleshooting Commands

## Check Nginx Status

```bash
sudo systemctl status nginx
```

## Check PHP-FPM Status

```bash
sudo systemctl status php-fpm
```

## Test Nginx Configuration

```bash
sudo nginx -t
```

## Check PHP Version

```bash
php -v
```

## Check PHP-FPM Version

```bash
php-fpm -v
```

## Check PHP-FPM Socket

```bash
ls -l /var/run/php-fpm/default.sock
```

## Check Nginx Port

```bash
sudo ss -tulnp | grep 8095
```

## Test Website Locally

```bash
curl http://localhost:8095/index.php
```

## Test from Jump Host

```bash
curl http://stapp01:8095/index.php
```

---

# Troubleshooting Process

The main troubleshooting flow I followed was:

```text
Install Nginx
      ↓
Configure Port 8095
      ↓
Restart Nginx
      ↓
Install PHP 8.2
      ↓
Install PHP-FPM
      ↓
Configure PHP-FPM Socket
      ↓
Configure Nginx PHP Location
      ↓
Restart PHP-FPM
      ↓
Restart Nginx
      ↓
Check Services
      ↓
Check Socket
      ↓
Run nginx -t
      ↓
Test with curl
```

I had to repeat the configuration process around **4–5 times** because of Nginx configuration errors, incorrect port configuration, PHP version issues, and PHP-FPM socket configuration problems.

---

# Important Lesson

This challenge taught me that when multiple services depend on each other, troubleshooting should be done step by step.

The final architecture was:

```text
Client
  ↓
Nginx :8095
  ↓
PHP-FPM 8.2
  ↓
Unix Socket
/var/run/php-fpm/default.sock
  ↓
PHP Application
/var/www/html/index.php
```

The most important verification commands were:

```bash
sudo nginx -t
```

```bash
sudo systemctl status nginx --no-pager
```

```bash
sudo systemctl status php-fpm
```

```bash
ls -l /var/run/php-fpm/default.sock
```

and finally:

```bash
curl http://stapp01:8095/index.php
```

**Day 19 completed successfully.** 🚀
