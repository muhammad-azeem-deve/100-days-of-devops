# Day 16 - Nginx Load Balancer Commands

This file contains the important commands used to complete **Day 16** of the KodeKloud 100 Days of DevOps Challenge.

---
Firstly, I access the LBR through ssh.
## 1. Check Nginx Version

```bash
nginx -v
```

Used to check whether Nginx is installed and display its version.

---

## 2. Check Nginx Service

```bash
sudo systemctl status nginx
```

Checks the current status of the Nginx service.

---

## 3. Install Nginx

If Nginx is not installed:

```bash
sudo dnf install nginx -y
```

---

## 4. Start Nginx

```bash
sudo systemctl start nginx
```

---

## 5. Enable Nginx at Boot

```bash
sudo systemctl enable nginx
```

---

## 6. Verify Application Server 1

```bash
curl http://stapp01:5003
```

Checks whether Apache on Application Server 1 is responding.

---

## 7. Verify Application Server 2

```bash
curl http://stapp02:5003
```

Checks whether Apache on Application Server 2 is responding.

---

## 8. Verify Application Server 3

```bash
curl http://stapp03:5003
```

Checks whether Apache on Application Server 3 is responding.

---

## 9. Start Apache

Run this on the application servers if Apache is not running:

```bash
sudo systemctl start httpd
```

---

## 10. Enable Apache

```bash
sudo systemctl enable httpd
```

---

## 11. Check Apache

```bash
sudo systemctl status httpd
```

Used to confirm that the Apache service is running.

---

## 12. Edit Main Nginx Configuration

The challenge specifically required modifying only the main Nginx configuration file:

```bash
sudo vi /etc/nginx/nginx.conf
```

Inside the `http` context, the load-balancing configuration was added:

```nginx
upstream app_servers {
    server stapp01:5003;
    server stapp02:5003;
    server stapp03:5003;
}

server {
    listen 80;
    server_name stlb01;

    location / {
        proxy_pass http://app_servers;
    }
}
```

---

## 13. Test Nginx Configuration

```bash
sudo nginx -t
```

This checks the Nginx configuration for syntax errors before applying the changes.

---

## 14. Reload Nginx

```bash
sudo systemctl reload nginx
```

Reloads the updated configuration without completely stopping the Nginx service.

---

## 15. Check Nginx Status

```bash
sudo systemctl status nginx
```

Confirms that Nginx is running after the configuration change.

---

## 16. Test the Load Balancer

```bash
curl http://stlb01:80
```

This accesses the application through the Nginx Load Balancer.

---

# Complete Command Sequence

```bash
# Check Nginx
nginx -v
sudo systemctl status nginx

# Install Nginx if required
sudo dnf install nginx -y

# Start and enable Nginx
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx

# Test application servers
curl http://stapp01:5003
curl http://stapp02:5003
curl http://stapp03:5003

# On application servers, make sure Apache is running
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd

# Edit the main Nginx configuration
sudo vi /etc/nginx/nginx.conf

# Test configuration
sudo nginx -t

# Reload Nginx
sudo systemctl reload nginx

# Verify Nginx
sudo systemctl status nginx

# Test the load balancer
curl http://stlb01:80
```

---

# Important Configuration

```nginx
upstream app_servers {
    server stapp01:5003;
    server stapp02:5003;
    server stapp03:5003;
}

server {
    listen 80;
    server_name stlb01;

    location / {
        proxy_pass http://app_servers;
    }
}
```

---

# Key Lesson

The most important point in this challenge was:

```text
Client → stlb01:80 → Nginx → stapp01/stapp02/stapp03:5003
```

The Apache port **5003** on the application servers was not changed. Nginx handled the incoming traffic on port **80** and distributed it among all three application servers.

Always use:

```bash
sudo nginx -t
```

before:

```bash
sudo systemctl reload nginx
```

This helps prevent applying an invalid Nginx configuration.
