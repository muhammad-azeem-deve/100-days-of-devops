# Day 82 - Commands

This file contains the commands used during **Day 82 of my 100 Days of DevOps Challenge**.

The objective was to create an INI-format Ansible inventory file for App Server 3 (`stapp03`) and execute an Ansible playbook using that inventory.

---

## Step 1: Access the Jump Host

Access the Jump Host using the `thor` user:

```bash
ssh thor@<jump-host>
```

---

## Step 2: Verify the Current User

```bash
whoami
```

Expected output:

```text
thor
```

---

## Step 3: Navigate to the Playbook Directory

```bash
cd /home/thor/playbook/
```

---

## Step 4: Check Directory Contents

```bash
ls -l
```

This displays the existing Ansible playbooks and other files.

---

## Step 5: Create the Inventory File

```bash
vi inventory
```

This creates the inventory file named `inventory`.

---

## Step 6: Add Server Information

Add the following INI-format configuration:

```ini
[app_servers]
stapp03 ansible_host=<SERVER_IP> ansible_user=<USERNAME> ansible_password=<PASSWORD>
```

Replace the placeholders with the actual IP address and SSH credentials provided by the lab.

Save and exit the file.

---

## Step 7: Verify the Inventory File

```bash
cat inventory
```

This displays the inventory configuration.

---

## Step 8: Execute the Ansible Playbook

```bash
ansible-playbook -i inventory playbook.yml
```

This command executes `playbook.yml` using the connection information stored in the `inventory` file.

---

## Step 9: Verify Inventory Structure

You can also check how Ansible reads the inventory:

```bash
ansible-inventory -i inventory --graph
```

This displays the inventory groups and their associated hosts.

Expected structure:

```text
@all:
  |--@app_servers:
  |  |--stapp03
```

---

# Quick Command Summary

```bash
# Verify current user
whoami

# Navigate to playbook directory
cd /home/thor/playbook/

# Check directory contents
ls -l

# Create inventory file
vi inventory

# Display inventory file
cat inventory

# Execute Ansible playbook
ansible-playbook -i inventory playbook.yml

# Verify inventory structure
ansible-inventory -i inventory --graph
```

---

# Understanding Inventory Variables

```ini
[app_servers]
stapp03 ansible_host=<SERVER_IP> ansible_user=<USERNAME> ansible_password=<PASSWORD>
```

| Variable | Purpose |
|----------|---------|
| `app_servers` | Inventory group name |
| `stapp03` | Target server hostname |
| `ansible_host` | Target server IP address |
| `ansible_user` | SSH login username |
| `ansible_password` | SSH login password |

---

# Important Ansible Commands

### 1. Display Inventory

```bash
ansible-inventory -i inventory --list
```

Displays the inventory in JSON format. Be careful when sharing this output because it may expose credentials.

### 2. Display Inventory Groups

```bash
ansible-inventory -i inventory --graph
```

Displays the groups and hosts configured in the inventory.

### 3. Execute Playbook

```bash
ansible-playbook -i inventory playbook.yml
```

Executes the playbook using the specified inventory.

---

# Important Lesson

An Ansible inventory tells Ansible **which servers to manage and how to connect to them**.

For this challenge, the most important configuration was:

```ini
[app_servers]
stapp03 ansible_host=<SERVER_IP> ansible_user=<USERNAME> ansible_password=<PASSWORD>
```

And the most important execution command was:

```bash
ansible-playbook -i inventory playbook.yml
```

The inventory file had to contain all the required connection information because the task required executing the playbook without additional connection arguments.

**Day 82 completed successfully.** 🚀

---

**Security Note:** The lab required a password inside the inventory file. In real-world projects, avoid committing plaintext passwords to GitHub. Use Ansible Vault or another secure secrets-management solution.
