# Day 20 - Deploy PHP Application with Nginx and PHP-FPM

## Challenge Overview

This is Day 20 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Nginx, PHP-FPM, PHP 8.2, Unix Sockets, and Web Server Configuration**.

The Nautilus application development team was preparing to launch a new PHP-based application on the Nautilus infrastructure in the Stratos Data Center. The task was to configure **Nginx and PHP-FPM** on **Application Server 1** so that the PHP application could be accessed successfully.

I had to install Nginx, configure it to listen on port `8095`, install PHP-FPM version `8.2`, configure PHP-FPM to use the required Unix socket, and then configure Nginx and PHP-FPM to work together.

During the task, I faced several issues including Nginx configuration errors, Nginx not listening on the correct port, and PHP/PHP-FPM version problems. I had to troubleshoot and redo the configuration around **4–5 times** before successfully completing the challenge.

### Challenge Requirement

The requirements were:

* Install `nginx` on **Application Server 1**.
* Configure Nginx to listen on port `8095`.
* Use `/var/www/html` as the document root.
* Install **PHP-FPM version 8.2**.
* Configure PHP-FPM to use the Unix socket:
  `/var/run/php-fpm/default.sock`
* Create the required parent directory if it does not exist.
* Configure Nginx and PHP-FPM to work together.
* Test the PHP application from the jump host using:

```bash
curl http://stapp01:8095/index.php
```

The provided `index.php` and `info.php` files were already present under `/var/www/html` and were **not modified**.

---

## Objectives

The objectives of this challenge are:

* Access Application Server 1 using SSH.
* Install and configure Nginx.
* Configure Nginx to listen on port `8095`.
* Set `/var/www/html` as the document root.
* Install PHP 8.2 and PHP-FPM.
* Configure PHP-FPM to use a Unix socket.
* Create `/var/run/php-fpm` if required.
* Configure the PHP-FPM pool.
* Connect Nginx with PHP-FPM.
* Verify PHP and PHP-FPM versions.
* Verify that the PHP-FPM socket exists.
* Validate the Nginx configuration.
* Test the PHP application using `curl`.
* Troubleshoot configuration and version-related errors.

---

## Environment

| Item              | Details                         |
| ----------------- | ------------------------------- |
| Challenge         | 100 Days of DevOps              |
| Platform          | KodeKloud                       |
| Day               | 20                              |
| Server            | Application Server 1            |
| Hostname          | `stapp01`                       |
| Web Server        | Nginx                           |
| Nginx Port        | `8095`                          |
| Document Root     | `/var/www/html`                 |
| PHP Version       | `8.2`                           |
| PHP-FPM           | `8.2`                           |
| PHP-FPM Socket    | `/var/run/php-fpm/default.sock` |
| Application Files | `index.php`, `info.php`         |
| Testing           | `curl`                          |
| Status            | Completed                       |

---

# Solution

## Step 1: Access Application Server 1

First, I accessed **Application Server 1** using SSH.

```bash
ssh <username>@stapp01
```

After connecting, I verified the server:

```bash
hostname
```

This ensured that I was working on the correct application server.

---

## Step 2: Install Nginx

I installed Nginx using:

```bash
sudo yum install nginx -y
```

After installation, I configured Nginx to use the required port and document root.

---

## Step 3: Configure Nginx

I opened the Nginx configuration file:

```bash
sudo vi /etc/nginx/nginx.conf
```

The Nginx configuration was adjusted so that:

* Nginx listens on port `8095`.
* The document root is `/var/www/html`.
* PHP requests are passed to the PHP-FPM Unix socket.

The PHP-FPM socket used was:

```text
/var/run/php-fpm/default.sock
```

---

## Step 4: Restart and Enable Nginx

After modifying the Nginx configuration, I restarted the service:

```bash
sudo systemctl restart nginx
```

I also enabled Nginx to start automatically after a reboot:

```bash
sudo systemctl enable nginx
```

Then I checked its status:

```bash
sudo systemctl status nginx
```

---

## Step 5: Install Required PHP Repository Packages

The default PHP packages did not provide the required PHP 8.2 environment, so I configured the Remi repository.

First, I installed EPEL:

```bash
sudo dnf install -y epel-release
```

Then I installed the Remi repository:

```bash
sudo dnf install -y https://rpms.remirepo.net/enterprise/remi-release-9.rpm
```

---

## Step 6: Configure PHP 8.2 Repository

I reset the existing PHP module:

```bash
sudo dnf module reset php -y
```

Then I enabled the PHP 8.2 module from Remi:

```bash
sudo dnf module enable php:remi-8.2 -y
```

This allowed me to install the required PHP 8.2 packages.

---

## Step 7: Install PHP 8.2 and PHP-FPM

I installed PHP, PHP-FPM, and the required PHP extensions:

```bash
sudo dnf install -y php php-fpm php-cli php-common php-mysqlnd php-gd php-mbstring php-xml php-zip php-json php-opcache
```

---

## Step 8: Verify PHP Version

After installation, I checked the PHP version:

```bash
php -v
```

The installed PHP version needed to be:

```text
PHP 8.2.x
```

I also verified the PHP-FPM version:

```bash
php-fpm -v
```

This confirmed that PHP-FPM was installed with the required PHP version.

---

## Step 9: Create PHP-FPM Socket Directory

The required PHP-FPM socket was:

```text
/var/run/php-fpm/default.sock
```

I created the parent directory:

```bash
sudo mkdir -p /var/run/php-fpm
```

The `-p` option creates the directory if it does not exist and does not produce an error if it already exists.

---

## Step 10: Configure PHP-FPM

I opened the PHP-FPM pool configuration:

```bash
sudo vi /etc/php-fpm.d/www.conf
```

I configured PHP-FPM to listen on the required Unix socket:

```text
listen = /var/run/php-fpm/default.sock
```

The PHP-FPM pool was configured so that Nginx could communicate with PHP-FPM through this socket.

---

## Step 11: Configure Nginx to Work with PHP-FPM

I opened the Nginx configuration again:

```bash
sudo vi /etc/nginx/nginx.conf
```

The PHP location configuration was set to pass `.php` requests to:

```text
/var/run/php-fpm/default.sock
```

This allows Nginx to serve normal files directly while forwarding PHP requests to PHP-FPM for processing.

---

## Step 12: Restart PHP-FPM

After updating the PHP-FPM configuration, I restarted the service:

```bash
sudo systemctl restart php-fpm
```

Then I verified the service:

```bash
sudo systemctl status php-fpm
```

---

## Step 13: Restart Nginx

After configuring both services, I restarted Nginx:

```bash
sudo systemctl restart nginx
```

I then checked the service:

```bash
sudo systemctl status nginx --no-pager
```

Both PHP-FPM and Nginx needed to be running correctly.

---

## Step 14: Verify PHP Versions Again

I verified PHP:

```bash
php -v
```

and PHP-FPM:

```bash
php-fpm -v
```

This was especially important because PHP version problems were one of the issues I encountered during the challenge.

---

## Step 15: Verify the PHP-FPM Socket

I checked whether the required Unix socket existed:

```bash
ls -l /var/run/php-fpm/default.sock
```

The socket needed to exist at:

```text
/var/run/php-fpm/default.sock
```

This confirmed that PHP-FPM had successfully created the socket.

---

## Step 16: Check Socket Ownership

During troubleshooting, I also checked and adjusted the socket ownership:

```bash
sudo chown -R nginx:nginx /var/run/php-fpm/default.sock
```

Then I verified it:

```bash
ls -l /var/run/php-fpm/default.sock
```

The ownership and permissions needed to allow Nginx to communicate with PHP-FPM.

---

## Step 17: Validate Nginx Configuration

Before testing the website, I validated the Nginx configuration:

```bash
sudo nginx -t
```

A successful result should indicate that the configuration syntax is valid.

This was an important step because I encountered Nginx configuration errors during my initial attempts.

---

## Step 18: Test the PHP Application

Finally, from the **jump host**, I tested the PHP application using:

```bash
curl http://stapp01:8095/index.php
```

The PHP application responded successfully, confirming that:

* Nginx was running.
* Nginx was listening on port `8095`.
* `/var/www/html` was being used as the document root.
* PHP 8.2 was installed.
* PHP-FPM was running.
* The PHP-FPM Unix socket was working.
* Nginx and PHP-FPM were communicating correctly.

---

# Troubleshooting

This challenge required several attempts before I successfully completed it.

I faced issues related to:

### 1. Nginx Not Running on the Correct Port

Initially, Nginx was not correctly configured to listen on port `8095`.

I checked and corrected the Nginx configuration:

```bash
sudo vi /etc/nginx/nginx.conf
```

After making changes, I restarted Nginx:

```bash
sudo systemctl restart nginx
```

---

### 2. Nginx Configuration Errors

I also encountered configuration errors while modifying `/etc/nginx/nginx.conf`.

I used:

```bash
sudo nginx -t
```

to validate the configuration before continuing.

This helped me identify and correct configuration problems.

---

### 3. PHP Version Problems

Another issue was installing the required PHP version.

The task specifically required:

```text
PHP 8.2
```

I therefore configured the Remi repository and enabled:

```text
php:remi-8.2
```

using:

```bash
sudo dnf module reset php -y
sudo dnf module enable php:remi-8.2 -y
```

I then verified the installation:

```bash
php -v
php-fpm -v
```

---

### 4. PHP-FPM Socket Problems

The application required PHP-FPM to use:

```text
/var/run/php-fpm/default.sock
```

I created the parent directory:

```bash
sudo mkdir -p /var/run/php-fpm
```

and configured PHP-FPM:

```text
listen = /var/run/php-fpm/default.sock
```

I then verified that the socket was created:

```bash
ls -l /var/run/php-fpm/default.sock
```

---

### 5. Multiple Attempts

I had to redo and troubleshoot the task around **4–5 times**.

Each attempt helped me understand the relationship between:

```text
Nginx
   ↓
PHP-FPM
   ↓
Unix Socket
   ↓
PHP Application
```

After correcting the Nginx configuration, PHP version, PHP-FPM socket, and service configuration, I successfully completed the task.

---

# Architecture

The final setup worked approximately as follows:

```text
                 Jump Host
                     |
                     | HTTP :8095
                     ↓
              ┌─────────────┐
              │    Nginx    │
              │   stapp01   │
              │    :8095    │
              └──────┬──────┘
                     |
                     | PHP Request
                     ↓
       /var/run/php-fpm/default.sock
                     |
                     ↓
              ┌─────────────┐
              │  PHP-FPM    │
              │    8.2      │
              └──────┬──────┘
                     |
                     ↓
              /var/www/html
                     |
             ┌───────┴───────┐
             ↓               ↓
         index.php        info.php
```

---

# What I Learned

From this challenge, I learned and practiced:

* How to install and configure Nginx.
* How to change the Nginx listening port.
* How to configure `/var/www/html` as a document root.
* How to install PHP 8.2 on a RHEL-based Linux environment.
* How to use EPEL and Remi repositories.
* How to install PHP-FPM and PHP extensions.
* How to configure PHP-FPM to use a Unix socket.
* How Nginx communicates with PHP-FPM.
* How to configure Nginx to process PHP files.
* How to check PHP and PHP-FPM versions.
* How to verify the PHP-FPM Unix socket.
* How to troubleshoot Nginx configuration errors.
* How to use `nginx -t` before restarting Nginx.
* How to use `systemctl` to manage Nginx and PHP-FPM.
* How to test a web application using `curl`.
* How important it is to troubleshoot systematically when multiple services depend on each other.

# Challenge Status

Day 20 — Completed Successfully 

**4–5 attempts, multiple Nginx and PHP-FPM errors, version issues, socket configuration problems, and finally a successful PHP application deployment.**

100 Days. 100 Challenges. One DevOps Journey. 

I will continue documenting each challenge.
