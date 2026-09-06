# Day 13 - IPtables Firewall Configuration Commands

This file contains the commands used to complete **Day 13** of the KodeKloud **100 Days of DevOps Challenge**.

---

## 1. SSH into the Application Server

Access each application server individually:

```bash
ssh tony@stapp01
```

Then repeat for:

```bash
ssh steve@stapp02
ssh banner@stapp03
```

---

## 2. Install IPtables

Install IPtables and its service package:

```bash
sudo dnf install -y iptables iptables-services
```

---

## 3. Check IPtables Version

```bash
iptables --version
```

---

## 4. Check Apache Service

```bash
systemctl status httpd
```

---

## 5. Check Listening Port

For the challenge's Apache port:

```bash
ss -lntp | grep 8086
```

If the lab environment is using another port, check that port instead.

---

## 6. Find Load Balancer IP

```bash
getent hosts stlb01
```

Example:

```text
<LBR_IP> stlb01
```

---

## 7. Check Existing IPtables Rules

```bash
iptables -L INPUT -n --line-numbers
```

---

## 8. Allow Load Balancer

Replace `<LBR_IP>` with the IP address of `stlb01`:

```bash
iptables -I INPUT 1 -p tcp -s <LBR_IP> --dport 8086 -j ACCEPT
```

### Explanation

* `-I INPUT 1` → Insert rule at position 1.
* `-p tcp` → Apply to TCP traffic.
* `-s <LBR_IP>` → Allow traffic from the Load Balancer.
* `--dport 8086` → Apply to port 8086.
* `-j ACCEPT` → Allow the traffic.

---

## 9. Block Everyone Else

```bash
iptables -I INPUT 2 -p tcp --dport 8086 -j DROP
```

### Explanation

This rule blocks TCP traffic to port `8086` from all other sources.

---

## 10. Verify Rule Order

```bash
iptables -L INPUT -n --line-numbers
```

Expected logic:

```text
1  ACCEPT  <LBR_IP>  tcp  dpt:8086
2  DROP    0.0.0.0/0  tcp  dpt:8086
```

The ACCEPT rule must appear before the DROP rule.

---

## 11. Save Firewall Rules

```bash
iptables-save > /etc/sysconfig/iptables
```

---

## 12. Enable IPtables at Boot

```bash
systemctl enable iptables
```

---

## 13. Restart IPtables

```bash
systemctl restart iptables
```

---

## 14. Verify Saved Rules

```bash
grep 8086 /etc/sysconfig/iptables
```

---

## 15. Verify IPtables Service

```bash
systemctl status iptables
```

---

## 16. Test Apache Locally

```bash
curl http://localhost:8086
```

---

## 17. Test from Load Balancer

From the LBR host:

```bash
curl http://stapp01:8086
curl http://stapp02:8086
curl http://stapp03:8086
```

The LBR should be able to access all three application servers.

---

# Complete Command Sequence

Run the following commands on each application server:

```bash
sudo dnf install -y iptables iptables-services

iptables --version

systemctl status httpd

ss -lntp | grep 8086

getent hosts stlb01

iptables -L INPUT -n --line-numbers

iptables -I INPUT 1 -p tcp -s <LBR_IP> --dport 8086 -j ACCEPT

iptables -I INPUT 2 -p tcp --dport 8086 -j DROP

iptables -L INPUT -n --line-numbers

iptables-save > /etc/sysconfig/iptables

systemctl enable iptables

systemctl restart iptables

grep 8086 /etc/sysconfig/iptables

curl http://localhost:8086
```

Then test from the Load Balancer:

```bash
curl http://stapp01:8086
curl http://stapp02:8086
curl http://stapp03:8086
```

---

# Quick Summary

```text
Install IPtables
      ↓
Find LBR IP
      ↓
Allow LBR → Port 8086
      ↓
Drop everyone else → Port 8086
      ↓
Verify rules
      ↓
Save rules
      ↓
Enable IPtables
      ↓
Restart IPtables
      ↓
Test Apache
```

---

# Important Lesson

The most important part of this task was the **order of IPtables rules**.

The Load Balancer must be allowed first:

```bash
iptables -I INPUT 1 -p tcp -s <LBR_IP> --dport 8086 -j ACCEPT
```

Then everyone else must be blocked:

```bash
iptables -I INPUT 2 -p tcp --dport 8086 -j DROP
```

If the DROP rule is evaluated before the ACCEPT rule, the Load Balancer will also be blocked.

---

## Day 13 Status

**Completed Successfully **

**Firewall:** IPtables
**Protected Port:** 8086
**Allowed:** Load Balancer
**Blocked:** Everyone else
**Persistent After Reboot:** Yes
