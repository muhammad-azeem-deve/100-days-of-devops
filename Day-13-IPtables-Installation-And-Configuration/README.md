# Day 13 - Configure IPtables Firewall on App Servers

## Challenge Overview

This is **Day 13** of my **100 Days of DevOps Challenge by KodeKloud**.

In this challenge, the security team identified that Apache's port **8086** was open to everyone because there was no firewall configured on the application hosts.

The task was to install and configure **iptables** on all application servers and restrict access to Apache so that only the **Load Balancer (LBR)** could access the Apache port.

### Challenge Requirements

1. Install **iptables** and all required dependencies on each application host.
2. Block incoming traffic on Apache port **8086** from everyone except the **LBR host**.
3. Make sure the firewall rules remain active even after a system reboot.

---

## Environment

| Component           | Details                                  |
| ------------------- | ---------------------------------------- |
| Datacenter          | Stratos DC                               |
| Firewall            | IPtables                                 |
| Application Servers | App Server 1, App Server 2, App Server 3 |
| Apache Port         | 8086                                     |
| Load Balancer       | `stlb01`                                 |
| Firewall Package    | `iptables`, `iptables-services`          |

---

## Objectives

* Install and configure IPtables.
* Identify the IP address of the Load Balancer.
* Allow the LBR to access Apache on port `8086`.
* Block all other incoming connections to port `8086`.
* Save firewall rules permanently.
* Enable IPtables at system boot.
* Verify that Apache remains accessible locally.
* Test access from the Load Balancer.

---

# Solution

## Step 1 - Access Each Application Server

The first step was to access each application server individually through SSH.

For example:

```bash
ssh <user>@stapp01
```

The same process was repeated for:

```text
stapp01
stapp02
stapp03
```

---

## Step 2 - Install IPtables

Install `iptables` and `iptables-services` on each application server.

```bash
sudo dnf install -y iptables iptables-services
```

The `iptables-services` package is important because it provides the service functionality required to restore saved firewall rules after reboot.

---

## Step 3 - Verify IPtables Installation

Check the installed IPtables version:

```bash
iptables --version
```

This confirmed that IPtables was successfully installed.

---

## Step 4 - Check Apache

Before configuring the firewall, I verified the Apache service:

```bash
systemctl status httpd
```

I also checked the listening ports:

```bash
ss -lntp | grep 5003
```

> **Note:** The challenge requirement mentions Apache port **8086**. If your lab environment shows a different port such as `5003`, always use the port specified by the actual challenge/environment.

---

## Step 5 - Find the Load Balancer IP

The firewall needs to allow traffic from the Load Balancer only.

I used:

```bash
getent hosts stlb01
```

This returned the IP address associated with the Load Balancer.

Example:

```text
<IP_ADDRESS> stlb01
```

The returned IP was then used in the IPtables rule.

---

## Step 6 - Check Existing Firewall Rules

Before adding new rules, I checked the existing INPUT chain:

```bash
iptables -L INPUT -n --line-numbers
```

This helped me understand the current firewall configuration and rule order.

---

## Step 7 - Allow the Load Balancer

I added an INPUT rule to allow the LBR IP to access Apache on port `8086`:

```bash
iptables -I INPUT 1 -p tcp -s <LBR_IP> --dport 8086 -j ACCEPT
```

Replace:

```text
<LBR_IP>
```

with the actual IP address returned by:

```bash
getent hosts stlb01
```

The rule allows TCP traffic on port `8086` only when the source is the Load Balancer.

---

## Step 8 - Block Everyone Else

After allowing the Load Balancer, I added a DROP rule:

```bash
iptables -I INPUT 2 -p tcp --dport 8086 -j DROP
```

This blocks incoming TCP traffic to port `8086` from all other sources.

### Why Rule Order Matters

The ACCEPT rule for the Load Balancer must be evaluated before the DROP rule.

The final order should look similar to:

```text
1  ACCEPT  tcp  --  <LBR_IP>  0.0.0.0/0  tcp dpt:8086
2  DROP    tcp  --  0.0.0.0/0  0.0.0.0/0  tcp dpt:8086
```

Therefore:

```text
LBR → ACCEPT
Everyone else → DROP
```

---

## Step 9 - Verify Firewall Rules

Check the INPUT chain again:

```bash
iptables -L INPUT -n --line-numbers
```

I verified that the ACCEPT rule for the LBR appeared before the DROP rule.

---

## Step 10 - Save IPtables Rules Permanently

To make sure the firewall configuration survives a reboot:

```bash
iptables-save > /etc/sysconfig/iptables
```

This saves the current firewall rules into the IPtables configuration file.

---

## Step 11 - Enable IPtables at Boot

Enable the IPtables service:

```bash
systemctl enable iptables
```

This ensures that the saved firewall rules are restored when the server starts.

---

## Step 12 - Restart IPtables

Restart the service:

```bash
systemctl restart iptables
```

---

## Step 13 - Verify Saved Rules

I checked that the port `8086` rules were saved:

```bash
grep 8086 /etc/sysconfig/iptables
```

The ACCEPT and DROP rules should be present.

---

## Step 14 - Test Apache Locally

I verified that Apache was still working locally:

```bash
curl http://localhost:8086
```

Apache should return the website/application response.

---

## Step 15 - Test Access from the Load Balancer

Finally, I tested access from the Load Balancer:

```bash
curl http://stapp01:8086
curl http://stapp02:8086
curl http://stapp03:8086
```

The Load Balancer should be able to access Apache because its IP address was explicitly allowed by the firewall.

---

# Important Firewall Logic

The final configuration follows this logic:

```text
                 Incoming Traffic
                       |
                       v
                Port 8086 ?
                       |
              +--------+--------+
              |                 |
          From LBR?          Other Source
              |                 |
              v                 v
           ACCEPT              DROP
```

This provides a simple security layer around the Apache service.

---

# Troubleshooting

### 1. Apache is not accessible

Check whether Apache is running:

```bash
systemctl status httpd
```

Check whether the expected port is listening:

```bash
ss -lntp | grep 8086
```

---

### 2. Firewall rules are not working

Check the rule order:

```bash
iptables -L INPUT -n --line-numbers
```

The LBR's ACCEPT rule must come before the DROP rule.

---

### 3. LBR cannot access Apache

Verify the Load Balancer IP:

```bash
getent hosts stlb01
```

Then check that the same IP exists in the ACCEPT rule.

---

### 4. Rules disappear after reboot

Check whether the rules were saved:

```bash
grep 8086 /etc/sysconfig/iptables
```

Check whether IPtables is enabled:

```bash
systemctl is-enabled iptables
```

If necessary:

```bash
systemctl enable iptables
```

---

# What I Learned

Through this challenge, I learned:

* How to install and configure **IPtables**.
* How INPUT firewall rules work.
* How to allow traffic from a specific IP address.
* How to block a specific port for all other sources.
* Why **firewall rule order** is important.
* How to save IPtables rules permanently.
* How to enable IPtables during system boot.
* How to verify firewall configuration using `iptables`, `ss`, and `curl`.
* How a firewall can provide an additional security layer for application servers.

---

# Final Result

The IPtables firewall was successfully configured on all application servers.

### Final Configuration

```text
Apache Port: 8086

Load Balancer:
    ALLOWED

Everyone Else:
    BLOCKED

Firewall:
    IPtables

Persistence:
    Enabled
```

The firewall rules were saved and configured to remain active after a system reboot.

---

## Status

**Day 13 Completed Successfully! **

**Topic:** IPtables Firewall Configuration
**Environment:** Stratos DC
**Servers:** App Server 1, App Server 2, App Server 3
**Protected Port:** 8086
**Allowed Source:** Load Balancer (`stlb01`)
