# Day 15 - Nginx + SSL Configuration Commands

This file contains the useful commands I used to complete **Day 15 of the KodeKloud 100 Days of DevOps Challenge**.

---

## 1. Install Nginx

```bash
sudo dnf install -y nginx
```

Verify the installation:

```bash
nginx --version
```

---

## 2. Enable and Start Nginx

```bash
sudo systemctl enable --now nginx
```

---

## 3. Create SSL Directory

```bash
sudo mkdir -p /etc/nginx/ssl
```

---

## 4. Move SSL Certificate and Key

```bash
sudo mv /tmp/nautilus.crt /etc/nginx/ssl/
sudo mv /tmp/nautilus.key /etc/nginx/ssl/
```

---

## 5. Secure the Private Key

```bash
sudo chmod 600 /etc/nginx/ssl/nautilus.key
```

---

## 6. Create Nginx SSL Configuration

```bash
sudo nano /etc/nginx/conf.d/ssl.conf
```

Example configuration:

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

## 7. Create the Website

```bash
echo 'Welcome!' | sudo tee /usr/share/nginx/html/index.html
```

Verify the content:

```bash
cat /usr/share/nginx/html/index.html
```

Expected:

```text
Welcome!
```

---

## 8. Backup Nginx Configuration

Before making changes to the main Nginx configuration, I created a backup:

```bash
sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak
```

---

## 9. Test Nginx Configuration

```bash
sudo nginx -t
```

This checks the Nginx configuration for syntax or configuration errors.

---

## 10. Restart Nginx

```bash
sudo systemctl restart nginx
```

---

## 11. Check Nginx Status

```bash
sudo systemctl status nginx
```

---

## 12. Verify SSL Files

```bash
sudo ls -l /etc/nginx/ssl/
```

---

## 13. Verify HTTPS Port

```bash
sudo ss -lntp | grep ':443'
```

---

## 14. Test HTTPS Locally

Because the certificate is self-signed, use `-k`:

```bash
curl -kI https://localhost/
```

Test the actual webpage:

```bash
curl -k https://localhost/
```

Expected output:

```text
Welcome!
```

---

## Quick Command Sequence

For quick reference:

```bash
sudo dnf install -y nginx

nginx --version

sudo systemctl enable --now nginx

sudo mkdir -p /etc/nginx/ssl

sudo mv /tmp/nautilus.crt /etc/nginx/ssl/
sudo mv /tmp/nautilus.key /etc/nginx/ssl/

sudo chmod 600 /etc/nginx/ssl/nautilus.key

sudo nano /etc/nginx/conf.d/ssl.conf

echo 'Welcome!' | sudo tee /usr/share/nginx/html/index.html

sudo nginx -t

sudo systemctl restart nginx

sudo systemctl status nginx

sudo ss -lntp | grep ':443'

curl -kI https://localhost/

curl -k https://localhost/
```

---

## Important Commands to Remember

| Command                  | Purpose                                 |
| ------------------------ | --------------------------------------- |
| `dnf install`            | Install Nginx                           |
| `systemctl enable --now` | Enable and start Nginx                  |
| `mkdir -p`               | Create SSL directory                    |
| `mv`                     | Move certificate and key                |
| `chmod 600`              | Secure private key                      |
| `nginx -t`               | Test Nginx configuration                |
| `systemctl restart`      | Restart Nginx                           |
| `systemctl status`       | Check Nginx status                      |
| `ss -lntp`               | Check listening ports                   |
| `curl -k`                | Test HTTPS with self-signed certificate |

---

## Key Lesson

The most important troubleshooting step in this task was:

```bash
sudo nginx -t
```

Always test the Nginx configuration before restarting the service. This helps identify configuration errors before they cause the Nginx service to fail.
