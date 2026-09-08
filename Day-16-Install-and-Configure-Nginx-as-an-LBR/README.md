# Day 16 - Configure Nginx Load Balancer

## Challenge Overview

This is **Day 16** of my **100 Days of DevOps Challenge by KodeKloud**.

In this challenge, the website traffic of the Nautilus production environment was increasing, which caused a degradation in website performance.

To improve performance and availability, the application was being migrated to a **High Availability stack**. The remaining task was to configure the **LBR (Load Balancer) server** in the Stratos Data Center.

The LBR server needed to distribute incoming website traffic across all available application servers.

---

## Challenge Requirement

Configure the **Nginx Load Balancer (LBR)** according to the following requirements:

1. Install **Nginx** on the LBR server if it is not already installed.
2. Configure Nginx as a load balancer using all application servers.
3. Configure load balancing inside the **HTTP context**.
4. Update **only** the main Nginx configuration file:

   ```text
   /etc/nginx/nginx.conf
   ```
5. Do not change the Apache port configured on the application servers.
6. Make sure Apache is running on all application servers.
7. Verify the website through:

   ```bash
   curl http://stlb01:80
   ```

---

## Environment

| Component              | Details                 |
| ---------------------- | ----------------------- |
| Load Balancer          | `stlb01`                |
| Load Balancer Software | Nginx                   |
| Application Server 1   | `stapp01`               |
| Application Server 2   | `stapp02`               |
| Application Server 3   | `stapp03`               |
| Apache Port            | `5003`                  |
| LBR Port               | `80`                    |
| Main Nginx Config      | `/etc/nginx/nginx.conf` |

---

## Architecture

```text
                    Client
                      |
                      | HTTP :80
                      v
               +--------------+
               |    stlb01    |
               |    Nginx     |
               | Load Balancer|
               +------+-------+
                      |
          +-----------+-----------+
          |           |           |
          v           v           v
     +---------+ +---------+ +---------+
     | stapp01 | | stapp02 | | stapp03 |
     | Apache  | | Apache  | | Apache  |
     |  :5003  | |  :5003  | |  :5003  |
     +---------+ +---------+ +---------+
```

Nginx receives requests on port **80** and distributes them among the three application servers on their existing Apache port **5003**.

---

# Solution Steps

## Step 1 - Check Nginx

First, check whether Nginx is installed on the LBR server.

```bash
nginx -v
```

Then check the Nginx service:

```bash
sudo systemctl status nginx
```

If Nginx is not installed, install it using:

```bash
sudo dnf install nginx -y
```

---

## Step 2 - Start and Enable Nginx

Start the Nginx service:

```bash
sudo systemctl start nginx
```

Enable it to start automatically after reboot:

```bash
sudo systemctl enable nginx
```

Verify the service:

```bash
sudo systemctl status nginx
```

---

## Step 3 - Verify Application Servers

Before configuring the load balancer, verify that all application servers are responding on their existing Apache port.

The Apache port provided by the challenge was **5003**, so I tested:

```bash
curl http://stapp01:5003
```

```bash
curl http://stapp02:5003
```

```bash
curl http://stapp03:5003
```

All application servers should return the website/application response.

> **Important:** The Apache port `5003` should not be changed. Nginx must forward traffic to the existing Apache port.

---

## Step 4 - Make Sure Apache Is Running

Apache must be running on all application servers.

On the respective application servers, Apache can be started with:

```bash
sudo systemctl start httpd
```

Enable Apache at boot:

```bash
sudo systemctl enable httpd
```

Verify its status:

```bash
sudo systemctl status httpd
```

This should be checked on all three application servers.

---

## Step 5 - Configure Nginx Load Balancing

The challenge specifically required modifying only:

```text
/etc/nginx/nginx.conf
```

Open the main Nginx configuration file:

```bash
sudo vi /etc/nginx/nginx.conf
```

Inside the `http` context, configure an upstream group containing all three application servers.

The load-balancing configuration should follow this structure:

```nginx
http {

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

}
```

### What this configuration does

The `upstream` block defines the application servers:

```nginx
upstream app_servers {
    server stapp01:5003;
    server stapp02:5003;
    server stapp03:5003;
}
```

Nginx distributes requests between these servers.

The LBR listens on port `80`:

```nginx
listen 80;
```

Requests received by Nginx are forwarded to the upstream application servers:

```nginx
proxy_pass http://app_servers;
```

---

## Step 6 - Test the Nginx Configuration

After saving `/etc/nginx/nginx.conf`, test the configuration before reloading Nginx:

```bash
sudo nginx -t
```

A successful configuration should show a message similar to:

```text
syntax is ok
test is successful
```

If the configuration test fails, fix the configuration before reloading Nginx.

---

## Step 7 - Reload Nginx

Once the configuration test succeeds, reload Nginx:

```bash
sudo systemctl reload nginx
```

Check the service:

```bash
sudo systemctl status nginx
```

Nginx should be in the:

```text
active (running)
```

state.

---

## Step 8 - Test the Load Balancer

Finally, test the LBR using:

```bash
curl http://stlb01:80
```

If the application response is returned, the Nginx load balancer is successfully forwarding requests to the application servers.

---

# Troubleshooting

### Nginx command not found

If:

```bash
nginx -v
```

does not work because Nginx is not installed, install it:

```bash
sudo dnf install nginx -y
```

---

### Nginx configuration error

Always test the configuration before reloading:

```bash
sudo nginx -t
```

Do not reload Nginx until the configuration test succeeds.

---

### Application server not responding

Test each application server directly:

```bash
curl http://stapp01:5003
curl http://stapp02:5003
curl http://stapp03:5003
```

If one server does not respond, check Apache on that server:

```bash
sudo systemctl status httpd
```

Start it if necessary:

```bash
sudo systemctl start httpd
```

---

### Important Port Lesson

One of the important requirements of this challenge was **not to modify the existing Apache port**.

The application servers were already configured to use:

```text
5003
```

Therefore, Nginx was configured to forward requests to:

```text
stapp01:5003
stapp02:5003
stapp03:5003
```

while clients access the application through:

```text
stlb01:80
```

---

# What I Learned

Through this challenge, I learned:

* How to install and manage Nginx using `systemctl`.
* How an Nginx server can work as a **load balancer**.
* How to use an Nginx `upstream` block.
* How to distribute traffic across multiple backend servers.
* How to configure Nginx inside the `http` context.
* Why the backend application port should not be changed unnecessarily.
* How to verify backend servers using `curl`.
* How to validate Nginx configuration using `nginx -t`.
* The difference between **reload** and restarting a service.
* How a load balancer provides a single entry point while multiple application servers handle the traffic.

---

# Final Verification

The following checks confirmed that the setup was working:

```bash
sudo nginx -t
```

```bash
sudo systemctl status nginx
```

```bash
curl http://stlb01:80
```

The website was successfully accessible through the **Nginx Load Balancer**.

---

## Status

**Day 16 Completed Successfully!**

**Topic:** Nginx Load Balancing / High Availability

**LBR:** `stlb01`

**Backend Servers:** `stapp01`, `stapp02`, `stapp03`

**Backend Port:** `5003`

**Load Balancer Port:** `80`
