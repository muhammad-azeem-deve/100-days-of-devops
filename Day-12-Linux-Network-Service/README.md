# Day 12 - Troubleshoot Apache Service and Port 6100

## Challenge Overview

This is **Day 12** of my **100 Days of DevOps Challenge by KodeKloud**.

The monitoring system reported an issue in the **Stratos Datacenter**. Apache was not reachable on **port 6100** on one of the application servers.

The task was to troubleshoot the issue and make sure Apache was reachable from the **jump host** without compromising the server's security.

### Challenge Requirement

The main requirements were:

* Troubleshoot why Apache was not reachable on port `6100`.
* Check whether Apache was listening on the required port.
* Check the firewall configuration.
* Fix the issue without modifying the existing `index.html`.
* Allow access to Apache from the jump host.
* Verify the service using:

```bash
curl http://stapp03:6100
```

## Environment

| Component    | Details              |
| ------------ | -------------------- |
| Challenge    | Day 12               |
| Platform     | KodeKloud            |
| Datacenter   | Stratos Datacenter   |
| Server       | Application Server 3 |
| Web Server   | Apache (`httpd`)     |
| Apache Port  | `6100`               |
| Source       | Jump Host            |
| Testing Tool | `curl`, `netstat`    |
| Firewall     | `iptables`           |

---

## Troubleshooting Process

### 1. Install Netstat Tools

First, I installed the `net-tools` package so that I could use `netstat` for troubleshooting.

```bash
dnf install net-tools -y
```

---

### 2. Check Listening Ports

I checked the ports currently listening on the server:

```bash
netstat -tunlp
```

Then I specifically checked whether anything was listening on port `6100`:

```bash
netstat -tunlp | grep 6100
```

This helped identify whether Apache was actually listening on the required port.

---

### 3. Test Apache Locally

I tested the Apache service directly from the application server:

```bash
curl localhost:6100
```

The service was not responding correctly, so I restarted Apache.

---

### 4. Restart Apache

I restarted the Apache service:

```bash
systemctl restart httpd
```

Then tested the port again:

```bash
curl localhost:6100
```

After restarting Apache, the local request started working.

---

### 5. Check Firewall Rules

Since the requirement also stated that Apache must be reachable from the jump host, I checked the firewall rules.

```bash
iptables -L
```

The required access to port `6100` from the jump host was not allowed.

---

### 6. Allow Jump Host Access to Port 6100

I added an `iptables` rule to allow TCP traffic to port `6100` specifically from the jump host:

```bash
iptables -I INPUT -p tcp -s 10.244.97.130 --dport 6100 -j ACCEPT
```

This was a better approach than opening port `6100` to everyone because the rule only allows traffic from the required jump host IP.

I verified the rule:

```bash
iptables -L
```

---

### 7. Save the Firewall Configuration

To make sure the firewall rule was preserved, I saved the current `iptables` configuration:

```bash
iptables-save > /etc/sysconfig/iptables
```

Then I restarted the iptables service:

```bash
systemctl restart iptables
```

And enabled it:

```bash
systemctl enable iptables
```

---

## Final Verification

After fixing Apache and the firewall configuration, the service was ready to be tested from the jump host.

From the jump host:

```bash
curl http://stapp03:6100
```

The Apache page was successfully reachable.

> **Important:** I did not modify the existing `index.html` file because the challenge specifically warned that changing it would cause the task to fail.

---

## Useful Troubleshooting Commands

The most important commands used during this challenge were:

```bash
dnf install net-tools -y
netstat -tunlp
netstat -tunlp | grep 6100
curl localhost:6100
systemctl restart httpd
iptables -L
iptables -I INPUT -p tcp -s 10.244.97.130 --dport 6100 -j ACCEPT
iptables-save > /etc/sysconfig/iptables
systemctl restart iptables
systemctl enable iptables
curl http://stapp03:6100
```

---

## What I Learned

Through this challenge, I learned how to troubleshoot a web server that is not reachable on a specific port.

### Key Lessons

* How to check listening ports using `netstat`.
* How to verify whether Apache is listening on the required port.
* How to test a web service locally using `curl`.
* How to restart the Apache `httpd` service.
* How to inspect firewall rules using `iptables`.
* How to allow traffic from a specific source IP.
* How to save and persist firewall rules.
* How firewall rules can prevent a working service from being accessed remotely.
* Why restricting access to a specific source is better than unnecessarily opening a port to everyone.

## Final Status

**Day 12 completed successfully!**

Apache was restored on port `6100`, the firewall was configured to allow the required jump-host traffic, and the application was successfully tested using:

```bash
curl http://stapp03:6100
```

No changes were made to the existing `index.html`.
