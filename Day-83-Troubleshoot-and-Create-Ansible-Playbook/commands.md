# Day 83 - Commands

This file contains the commands used during **Day 83 of my 100 Days of DevOps Challenge by KodeKloud**.

The objective was to update the Ansible inventory for App Server 2 (`stapp02`) and create an Ansible playbook that creates an empty file at `/tmp/file.txt`.

---

# Step 1: Access the Jump Host

Connect to the Jump Host:

```bash
ssh thor@<jump-host>
```

---

# Step 2: Navigate to the Ansible Directory

```bash
cd /home/thor/ansible
```

Check the existing files:

```bash
ls -l
```

---

# Step 3: Open the Inventory File

```bash
vi /home/thor/ansible/inventory
```

Inspect the inventory:

```bash
cat /home/thor/ansible/inventory
```

---

# Step 4: Update the Inventory

Replace the old server entry with App Server 2 connection details.

Inventory format:

```ini
stapp02 ansible_host=<APP_SERVER_2_IP> ansible_user=<SSH_USERNAME> ansible_password=<SSH_PASSWORD>
```

### Parameter Explanation

```text
stapp02                  → Inventory hostname
ansible_host             → Target server IP address
ansible_user             → SSH username
ansible_password         → SSH password
```

Save and exit the file.

---

# Step 5: Create the Playbook

Create the playbook file:

```bash
vi /home/thor/ansible/playbook.yml
```

Add the following YAML configuration:

```yaml
---
- name: Create an empty file on App Server 2
  hosts: stapp02
  tasks:
    - name: Create file.txt
      file:
        path: /tmp/file.txt
        state: touch
```

Save and exit.

---

# Step 6: Verify Inventory and Playbook

Check the inventory:

```bash
cat /home/thor/ansible/inventory
```

Check the playbook:

```bash
cat /home/thor/ansible/playbook.yml
```

List the files:

```bash
ls -l /home/thor/ansible
```

---

# Step 7: Execute the Playbook

Navigate to the working directory:

```bash
cd /home/thor/ansible
```

Execute the playbook:

```bash
ansible-playbook -i inventory playbook.yml
```

This command uses the inventory file for server connection details and executes the tasks defined in the playbook.

---

# Step 8: Verify Playbook Execution

Check the play recap displayed after execution.

Example:

```text
PLAY RECAP
stapp02 : ok=2 changed=1 unreachable=0 failed=0
```

The important values are:

```text
unreachable=0
failed=0
```

These indicate that the server was reachable and no task failed.

---

# Step 9: Verify the File on App Server 2

Connect to App Server 2:

```bash
ssh <SSH_USERNAME>@<APP_SERVER_2_IP>
```

Check whether the file exists:

```bash
ls -l /tmp/file.txt
```

Verify that the file is empty:

```bash
stat -c %s /tmp/file.txt
```

Expected output:

```text
0
```

---

# Quick Command Summary

```bash
# Access Jump Host
ssh thor@<jump-host>

# Navigate to Ansible directory
cd /home/thor/ansible

# Check existing files
ls -l

# Open inventory
vi inventory

# Check inventory
cat inventory

# Create playbook
vi playbook.yml

# Check playbook
cat playbook.yml

# Execute Ansible playbook
ansible-playbook -i inventory playbook.yml

# Connect to App Server 2
ssh <SSH_USERNAME>@<APP_SERVER_2_IP>

# Verify file
ls -l /tmp/file.txt

# Check file size
stat -c %s /tmp/file.txt
```

---

# Important Ansible Configuration

## Inventory File

Location:

```text
/home/thor/ansible/inventory
```

Configuration:

```ini
stapp02 ansible_host=<APP_SERVER_2_IP> ansible_user=<SSH_USERNAME> ansible_password=<SSH_PASSWORD>
```

## Playbook File

Location:

```text
/home/thor/ansible/playbook.yml
```

Configuration:

```yaml
---
- name: Create an empty file on App Server 2
  hosts: stapp02
  tasks:
    - name: Create file.txt
      file:
        path: /tmp/file.txt
        state: touch
```

---

# Important Ansible Commands

### Check Ansible Version

```bash
ansible --version
```

### List Inventory Hosts

```bash
ansible-inventory -i inventory --list
```

### Test Remote Connectivity

```bash
ansible stapp02 -i inventory -m ping
```

### Execute Playbook

```bash
ansible-playbook -i inventory playbook.yml
```

---

# Important Lesson

Ansible requires two important components:

**1. Inventory:** Defines which servers Ansible will manage and how to connect to them.

**2. Playbook:** Defines which tasks Ansible should execute on those servers.

For this challenge, the inventory was configured for `stapp02`, and the playbook used the `file` module with `state: touch` to create an empty file.

The most important execution command was:

```bash
ansible-playbook -i inventory playbook.yml
```

**Day 83 completed successfully.** 🚀
