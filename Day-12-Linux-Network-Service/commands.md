# Day 12 - Apache Port 6100 Troubleshooting Commands

This file contains the important commands used to troubleshoot and fix the Apache connectivity issue on **Application Server 3**.

---

## 1. Install Netstat

Install `net-tools` to use `netstat`:

```bash
dnf install net-tools -y
```

---

## 2. Check Listening Ports

Display all listening TCP and UDP ports:

```bash
netstat -tunlp
```

Check specifically for port `6100`:

```bash
netstat -tunlp | grep 6100
```

---

## 3. Test Apache Locally

Test whether Apache responds on port `6100` from the application server:

```bash
curl localhost:6100
```

---

## 4. Restart Apache

Restart the Apache web server:

```bash
systemctl restart httpd
```

Test it again:

```bash
curl localhost:6100
```

---

## 5. Check Firewall Rules

List the current `iptables` rules:

```bash
iptables -L
```

---

## 6. Allow Jump Host Access

Allow TCP traffic to port `6100` only from the jump host:

```bash
iptables -I INPUT -p tcp -s 10.244.97.130 --dport 6100 -j ACCEPT
```

Verify the rule:

```bash
iptables -L
```

### Command Breakdown

```text
-I INPUT
```

Adds the rule to the INPUT chain.

```text
-p tcp
```

Allows TCP traffic.

```text
-s 10.244.97.130
```

Specifies the jump host as the allowed source IP.

```text
--dport 6100
```

Specifies Apache's port.

```text
-j ACCEPT
```

Allows the matching traffic.

---

## 7. Save Firewall Rules

Save the current `iptables` configuration:

```bash
iptables-save > /etc/sysconfig/iptables
```

---

## 8. Restart and Enable iptables

Restart the firewall service:

```bash
systemctl restart iptables
```

Enable it at boot:

```bash
systemctl enable iptables
```

---

## 9. Final Test from Jump Host

From the jump host, verify that Apache is reachable:

```bash
curl http://stapp03:6100
```

---

# Quick Command Summary

```bash
dnf install net-tools -y

netstat -tunlp

netstat -tunlp | grep 6100

curl localhost:6100

systemctl restart httpd

curl localhost:6100

iptables -L

iptables -I INPUT -p tcp -s 10.244.97.130 --dport 6100 -j ACCEPT

iptables -L

iptables-save > /etc/sysconfig/iptables

systemctl restart iptables

systemctl enable iptables

curl http://stapp03:6100
```

---

# Important Lesson

The main troubleshooting approach for this challenge was:

```text
Check Port
    ↓
Test Apache Locally
    ↓
Restart Apache
    ↓
Check Firewall
    ↓
Allow Required Source IP
    ↓
Save Firewall Rules
    ↓
Test from Jump Host
```

The important point was to **allow only the required jump-host traffic** instead of unnecessarily opening port `6100` to all sources.

Also, the existing `index.html` was intentionally left unchanged as required by the challenge.
