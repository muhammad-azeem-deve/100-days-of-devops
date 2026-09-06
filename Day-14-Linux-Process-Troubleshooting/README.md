# Day 14 - Troubleshoot Apache Service and Port 3000

## Challenge Overview

This is **Day 14** of my **100 Days of DevOps Challenge by KodeKloud**.

The production support team of **xFusionCorp Industries** reported that the Apache service was unavailable on one of the application servers in the **Stratos Datacenter**.

The task was to identify the faulty application server, troubleshoot the Apache service, and make sure Apache was running correctly on **all application servers**.

### Challenge Requirement

* Identify the application server where Apache is not working.
* Make sure the Apache (`httpd`) service is running.
* Make sure Apache is listening on **port 3000**.
* Check for any process occupying port 3000.
* Stop the conflicting process if necessary.
* Restart Apache.
* Enable Apache to start automatically after reboot.
* Verify that Apache is running successfully on all application servers.

---

## Environment

| Component           | Details                                  |
| ------------------- | ---------------------------------------- |
| Datacenter          | Stratos DC                               |
| Service             | Apache / httpd                           |
| Application Servers | App Server 1, App Server 2, App Server 3 |
| Required Port       | `3000`                                   |
| Configuration File  | `/etc/httpd/conf/httpd.conf`             |
| Service Name        | `httpd`                                  |

---

## Step 1: Check Apache Status

First, I checked the Apache service on each application server.

```bash
systemctl status httpd
```

The purpose was to identify which server had the Apache service stopped or failed.

---

## Step 2: Check Port 3000

I checked whether Apache was listening on the required port.

```bash
ss -tuln | grep 3000
```

If Apache is correctly configured, port `3000` should appear in the listening sockets.

---

## Step 3: Check Apache Listen Configuration

I checked the Apache configuration to find which port Apache was configured to listen on.

```bash
grep -Rni "^[[:space:]]*Listen" /etc/httpd/
```

The configuration needed to contain:

```text
Listen 3000
```

If the port was incorrect, I edited the Apache configuration:

```bash
vi /etc/httpd/conf/httpd.conf
```

and changed the appropriate `Listen` directive to:

```apache
Listen 3000
```

---

## Step 4: Validate Apache Configuration

Before restarting Apache, I checked whether the configuration contained any syntax errors.

```bash
apachectl configtest
```

A successful configuration should return:

```text
Syntax OK
```


---

## Step 5: Check Apache Processes


I checked which process was using port 3000:

```bash
ss -lntp | grep :3000
```

This was important because another process could already be occupying port `3000`, preventing Apache from starting.

---

## Step 6: Stop the Conflicting Process

If another process was found using port `3000`, I identified its PID from the previous command.

The process can then be terminated using:

```bash
kill -9 <PID>
```

For example:

```bash
kill -9 105414
```

> The PID is only an example. Always use the PID of the actual conflicting process found on the server.

After stopping the process, I checked the port again:

```bash
ss -lntp | grep :3000
```

---

## Step 7: Restart Apache

Once the port was free and the Apache configuration was correct, I restarted Apache:

```bash
systemctl restart httpd
```

Then I checked its status:

```bash
systemctl status httpd
```

The expected result was:

```text
Active: active (running)
```

---

## Step 8: Enable Apache at Boot

To make sure Apache automatically starts after a server reboot:

```bash
systemctl enable httpd
```

I then started Apache if necessary:

```bash
systemctl start httpd
```

And verified the final status:

```bash
systemctl status httpd
```

---

## Step 9: Verify Port 3000

Finally, I confirmed that Apache was listening on port `3000`:

```bash
ss -lntp | grep :3000
```

The output should show Apache/httpd listening on port `3000`.

---

## Step 10: Test Apache

I also tested the Apache endpoint from another server:

```bash
curl http://stapp01:3000
```

The server did not necessarily need to have website content deployed. The main requirement was that the Apache service was running and listening on port `3000`.

---

## Troubleshooting

### Apache Service Was Not Running

I first checked:

```bash
systemctl status httpd
```

If it was stopped, I started it:

```bash
systemctl start httpd
```

or restarted it:

```bash
systemctl restart httpd
```

### Apache Was Not Listening on Port 3000

I checked the configuration:

```bash
grep -Rni "^[[:space:]]*Listen" /etc/httpd/
```

Then edited the configuration if required:

```bash
vi /etc/httpd/conf/httpd.conf
```

The required configuration was:

```apache
Listen 3000
```

### Port 3000 Was Already in Use

I checked the process using the port:

```bash
ss -lntp | grep :3000
```

After identifying the PID, I stopped the conflicting process:

```bash
kill -9 <PID>
```

Then I restarted Apache:

```bash
systemctl restart httpd
```

---

## Final Verification

The final checks were:

```bash
systemctl status httpd
```

```bash
ss -lntp | grep :3000
```

```bash
apachectl configtest
```

Expected results:

* Apache service: **active (running)**
* Apache enabled at boot: **yes**
* Apache listening port: **3000**
* Apache configuration: **Syntax OK**

---

## What I Learned

From this challenge, I learned how to troubleshoot an Apache service when it is unavailable.

### Key lessons:

* How to check the status of the `httpd` service.
* How to verify listening ports using `ss`.
* How to find Apache's `Listen` configuration.
* How to validate Apache configuration using `apachectl configtest`.
* How to identify processes using a specific port.
* How to terminate a conflicting process.
* How to restart and enable Apache using `systemctl`.
* How to verify that Apache is listening on the required port.

This challenge helped me understand that when a service is unavailable, the problem may not always be the service itself. A **wrong configuration or another process occupying the required port** can also prevent the service from working.

---

## Challenge Status

**Day 14 Completed Successfully **

Apache was configured and verified to run on **port 3000**, and the service was enabled to start automatically.
