
# Day 01 — Commands

> Commands used during the Linux User Setup with Non-Interactive Shell challenge.

---


## Command Summary
### 1. Connect to Server 1
ssh steve@stapp01

### 2. Create user with non-interactive shell
sudo useradd -s /sbin/nologin kareem

### 3. Verify user
getent passwd kareem

### 4. Verify through /etc/passwd
grep '^kareem:' /etc/passwd

### 5. Display assigned shell
getent passwd kareem | cut -d: -f7
