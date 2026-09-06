# Day 14 - Apache Troubleshooting Commands

## 1. Check Apache Service

Run this on each application server:

```bash
systemctl status httpd
```

Purpose:

* Check whether Apache is running.
* Identify the faulty application server.

---

## 2. Check Port 3000

```bash
ss -tuln | grep 3000
```

Purpose:

* Check whether port `3000` is listening.
* Verify whether Apache is configured to use the required port.

---

## 3. Find Apache Listen Configuration

```bash
grep -Rni "^[[:space:]]*Listen" /etc/httpd/
```

Expected configuration:

```text
Listen 3000
```

---

## 4. Edit Apache Configuration

If Apache is configured for the wrong port:

```bash
vi /etc/httpd/conf/httpd.conf
```

Set:

```apache
Listen 3000
```

Save and exit the file.

---

## 5. Validate Apache Configuration

```bash
apachectl configtest
```

Expected:

```text
Syntax OK
```

---

## 6. Check Effective Apache Listen Configuration

```bash
httpd -t -D DUMP_RUN_CFG | grep -i listen
```

This helps confirm the port Apache is actually configured to listen on.

---

## 7. Check Running Apache Processes

```bash
ps -ef | grep '[h]ttpd'
```

This displays the running Apache processes.

---

## 8. Find Which Process Is Using Port 3000

```bash
ss -lntp | grep :3000
```

This is especially useful if Apache cannot start because port `3000` is already occupied.

---

## 9. Kill the Conflicting Process

After identifying the PID:

```bash
kill -9 <PID>
```

Example:

```bash
kill -9 105414
```

> Replace `<PID>` with the actual PID found from `ss -lntp | grep :3000`.

---

## 10. Verify That Port 3000 Is Free

```bash
ss -lntp | grep :3000
```

If there is no conflicting process, restart Apache.

---

## 11. Restart Apache

```bash
systemctl restart httpd
```

---

## 12. Check Apache Status

```bash
systemctl status httpd
```

Expected:

```text
Active: active (running)
```

---

## 13. Enable Apache at Boot

```bash
systemctl enable httpd
```

This makes Apache start automatically after a system reboot.

---

## 14. Start Apache

```bash
systemctl start httpd
```

---

## 15. Final Apache Status Check

```bash
systemctl status httpd
```

---

## 16. Final Port Verification

```bash
ss -lntp | grep :3000
```

Apache/httpd should be listening on port `3000`.

---

## 17. Test Apache

From another server:

```bash
curl http://stapp01:3000
```

The important requirement is that the Apache service is available on port `3000`; website content is not required for this challenge.

---

# Quick Troubleshooting Sequence

The most useful commands for this task can be remembered in this order:

```bash
# 1. Check service
systemctl status httpd

# 2. Check port
ss -tuln | grep 3000

# 3. Check Apache Listen configuration
grep -Rni "^[[:space:]]*Listen" /etc/httpd/

# 4. Validate configuration
apachectl configtest

# 5. Check process using port 3000
ss -lntp | grep :3000

# 6. Check Apache processes
ps -ef | grep '[h]ttpd'

# 7. Kill conflicting process if required
kill -9 <PID>

# 8. Restart Apache
systemctl restart httpd

# 9. Enable Apache
systemctl enable httpd

# 10. Verify service
systemctl status httpd

# 11. Verify port
ss -lntp | grep :3000
```

# Important Lesson

When troubleshooting a service, don't immediately restart it.

A better approach is:

```text
Check Service
     ↓
Check Port
     ↓
Check Configuration
     ↓
Check Conflicting Process
     ↓
Fix the Problem
     ↓
Restart Service
     ↓
Enable Service
     ↓
Verify Service + Port
```

This systematic approach makes service troubleshooting much easier.
