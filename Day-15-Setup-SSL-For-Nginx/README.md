# Day 15 - Install and Configure Nginx with SSL

## Challenge Overview

This is **Day 15** of my **100 Days of DevOps Challenge by KodeKloud**.

In this challenge, the system administrators of **xFusionCorp Industries** needed to prepare **App Server 1** for a new application deployment.

The main task was to install and configure **Nginx**, deploy the provided self-signed SSL certificate, create a simple web page, and verify HTTPS access from the server.

---

## Challenge Requirements

The requirements were:

1. Install and configure **Nginx** on App Server 1.
2. Move the existing SSL certificate and private key:

   * `/tmp/nautilus.crt`
   * `/tmp/nautilus.key`
3. Configure the SSL certificate and key in Nginx.
4. Create an `index.html` file containing:

```text
Welcome!
```

5. Use `curl` from the jump host to test the App Server 1 hostname over HTTPS.

Example:

```bash
curl -Ik https://<app-server-name>/
```

---

## Environment

| Component        | Details                            |
| ---------------- | ---------------------------------- |
| Challenge        | Day 15                             |
| Server           | App Server 1                       |
| Web Server       | Nginx                              |
| Operating System | Linux                              |
| Package Manager  | DNF                                |
| SSL Certificate  | `/tmp/nautilus.crt`                |
| SSL Key          | `/tmp/nautilus.key`                |
| SSL Location     | `/etc/nginx/ssl/`                  |
| Document Root    | `/usr/share/nginx/html/`           |
| Web Page         | `/usr/share/nginx/html/index.html` |
| HTTPS Port       | `443`                              |

---

## Step 1 - Install Nginx

First, I installed Nginx using the DNF package manager.

```bash
sudo dnf install -y nginx
```

Then I verified the installation:

```bash
nginx --version
```

---

## Step 2 - Start and Enable Nginx

I enabled Nginx to start automatically after reboot and started it immediately:

```bash
sudo systemctl enable --now nginx
```

---

## Step 3 - Create an SSL Directory

I created a dedicated directory under `/etc/nginx` to store the SSL certificate and private key.

```bash
sudo mkdir -p /etc/nginx/ssl
```

---

## Step 4 - Move the SSL Certificate and Key

The challenge provided the certificate and key in `/tmp`.

I moved them to the Nginx SSL directory:

```bash
sudo mv /tmp/nautilus.crt /etc/nginx/ssl/
sudo mv /tmp/nautilus.key /etc/nginx/ssl/
```

For security, I restricted access to the private key:

```bash
sudo chmod 600 /etc/nginx/ssl/nautilus.key
```

---

## Step 5 - Configure SSL in Nginx

I created an SSL configuration file inside the Nginx `conf.d` directory:

```bash
sudo nano /etc/nginx/conf.d/ssl.conf
```

The configuration was used to enable HTTPS and point Nginx to the certificate and private key.

The important configuration values were:

```nginx
server {
    listen 443 ssl;
    server_name _;

    ssl_certificate /etc/nginx/ssl/nautilus.crt;
    ssl_certificate_key /etc/nginx/ssl/nautilus.key;

    root /usr/share/nginx/html;
    index index.html;
}
```

---

## Step 6 - Create the Website

I created the required `index.html` file in the Nginx document root.

```bash
echo 'Welcome!' | sudo tee /usr/share/nginx/html/index.html
```

The page can be checked with:

```bash
cat /usr/share/nginx/html/index.html
```

Expected output:

```text
Welcome!
```

---

## Step 7 - Check Nginx Configuration

Before restarting Nginx, I tested the configuration to make sure there were no syntax errors:

```bash
sudo nginx -t
```

A successful configuration should show messages similar to:

```text
syntax is ok
test is successful
```

---

## Step 8 - Restart and Verify Nginx

After the configuration test passed, I restarted Nginx:

```bash
sudo systemctl restart nginx
```

Then I checked the service status:

```bash
sudo systemctl status nginx
```

The service should be:

```text
active (running)
```

---

## Step 9 - Verify HTTPS Port

I verified that Nginx was listening on HTTPS port `443`:

```bash
sudo ss -lntp | grep ':443'
```

This confirmed that the HTTPS service was listening on port 443.

---

## Step 10 - Test HTTPS Locally

Because the certificate was self-signed, I used `-k` with curl to ignore certificate verification.

First, I tested the HTTPS headers:

```bash
curl -kI https://localhost/
```

Then I tested the actual page:

```bash
curl -k https://localhost/
```

Expected output:

```text
Welcome!
```

---

## What I Learned

From this challenge, I learned how to:

* Install and manage Nginx using DNF.
* Manage Nginx using `systemctl`.
* Configure HTTPS in Nginx.
* Deploy an SSL certificate and private key.
* Understand the importance of securing private key permissions.
* Configure the Nginx document root.
* Test Nginx configuration using `nginx -t`.
* Check listening ports using `ss`.
* Test HTTPS endpoints using `curl`.
* Work with self-signed SSL certificates using the `curl -k` option.

---

## Challenge Status

**Day 15 completed successfully!**

I successfully prepared App Server 1 with **Nginx + HTTPS/SSL** and verified that the website returned:

```text
Welcome!
```

This challenge improved my practical understanding of **Linux server administration, Nginx, SSL/TLS, and web server troubleshooting**.
