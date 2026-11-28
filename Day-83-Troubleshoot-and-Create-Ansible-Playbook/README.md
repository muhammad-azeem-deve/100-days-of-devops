# Day 83 - Troubleshoot and Create Ansible Playbook

## Challenge Overview

This is Day 83 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Ansible Inventory Configuration and Playbook Creation**. A team member had left an Ansible setup unfinished on the Jump Host, and the task was to complete the configuration for App Server 2 (`stapp02`) in Stratos DC.

The challenge required updating the existing inventory file with the correct server connection details and creating an Ansible playbook to create an empty file at `/tmp/file.txt` on App Server 2.

An important requirement was that all connection details had to be configured inside the inventory file because the validation command would execute the playbook without additional connection arguments.

### Challenge Requirement

Complete the unfinished Ansible configuration on the Jump Host.

The task involved:

1. Update the existing inventory file at `/home/thor/ansible/inventory`.
2. Replace the old server entry with App Server 2 (`stapp02`).
3. Configure the correct server IP address, SSH username, and password.
4. Create a playbook at `/home/thor/ansible/playbook.yml`.
5. Use the Ansible `file` module to create an empty file at `/tmp/file.txt`.
6. Execute the playbook using the provided inventory.
7. Verify that the playbook runs successfully.

---

## Objectives

The objectives of this challenge are:

- Access the Jump Host using the `thor` user.
- Navigate to the Ansible working directory.
- Inspect and update the existing inventory file.
- Configure App Server 2 as the target host.
- Add SSH connection details inside the inventory.
- Create an Ansible playbook using YAML.
- Understand the purpose of the `hosts` directive.
- Understand how the Ansible `file` module works.
- Use `state: touch` to create an empty file.
- Execute the playbook using `ansible-playbook`.
- Verify successful execution and file creation.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 83 |
| Working Server | Jump Host |
| Jump Host User | `thor` |
| Target Server | App Server 2 |
| Target Hostname | `stapp02` |
| Inventory File | `/home/thor/ansible/inventory` |
| Playbook File | `/home/thor/ansible/playbook.yml` |
| Ansible Module | `file` |
| Target File | `/tmp/file.txt` |
| Required State | Empty File |
| Status | Completed |

---

# Solution

## Step 1: Access the Jump Host

First, I accessed the Jump Host using the `thor` user.

```bash
ssh thor@<jump-host>
```

After connecting, I navigated to the Ansible working directory:

```bash
cd /home/thor/ansible
```

I checked the existing files:

```bash
ls -l
```

The directory contained the existing Ansible inventory file, which needed to be updated.

---

## Step 2: Inspect the Existing Inventory File

I opened the existing inventory file:

```bash
vi /home/thor/ansible/inventory
```

The inventory contained an old server entry that needed to be replaced.

The objective was to configure the inventory for App Server 2 (`stapp02`).

---

## Step 3: Update the Inventory File

I replaced the old server entry with the correct connection information for App Server 2.

The inventory configuration followed this format:

```ini
stapp02 ansible_host=<APP_SERVER_2_IP> ansible_user=<SSH_USERNAME> ansible_password=<SSH_PASSWORD>
```

### Explanation

- `stapp02`: Inventory hostname of App Server 2.
- `ansible_host`: IP address of App Server 2.
- `ansible_user`: SSH username used to connect to the server.
- `ansible_password`: SSH password used for authentication.

I saved the updated inventory file.

**Important:** The actual IP address, username, and password must be taken from the KodeKloud lab environment.

---

## Step 4: Create the Ansible Playbook

After updating the inventory, I created the required playbook:

```bash
vi /home/thor/ansible/playbook.yml
```

I added the following YAML configuration:

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

### Explanation

- `name`: Describes the playbook.
- `hosts: stapp02`: Specifies App Server 2 as the target server.
- `tasks`: Contains the operations to execute.
- `file`: Ansible module used to manage files.
- `path`: Specifies the location of the file.
- `state: touch`: Creates the file if it does not already exist.

The main requirement was to create an empty file at:

```text
/tmp/file.txt
```

I saved the playbook after adding the configuration.

---

## Step 5: Verify the Ansible Files

Before executing the playbook, I checked the working directory:

```bash
ls -l /home/thor/ansible
```

I also checked the inventory:

```bash
cat /home/thor/ansible/inventory
```

And reviewed the playbook:

```bash
cat /home/thor/ansible/playbook.yml
```

This helped ensure that both files were configured correctly.

---

## Step 6: Execute the Ansible Playbook

Next, I navigated to the Ansible directory:

```bash
cd /home/thor/ansible
```

I executed the playbook using the required validation command:

```bash
ansible-playbook -i inventory playbook.yml
```

This command performs the following operations:

1. Reads the inventory file.
2. Identifies App Server 2.
3. Uses the connection details stored in the inventory.
4. Connects to the remote server.
5. Executes the playbook tasks.
6. Creates `/tmp/file.txt` on App Server 2.

---

## Step 7: Verify the Playbook Execution

After executing the playbook, I checked the output to ensure that the task completed successfully.

A successful execution should show a play recap similar to:

```text
PLAY RECAP
stapp02 : ok=2 changed=1 unreachable=0 failed=0
```

The exact task counts may vary depending on the playbook.

The important indicators are:

- `unreachable=0`: Ansible successfully connected to the target server.
- `failed=0`: No task failed.
- `changed`: Indicates whether Ansible made a change.

---

## Step 8: Verify the File on App Server 2

Finally, I verified that the file was created on the target server.

The file location was:

```text
/tmp/file.txt
```

To check the file:

```bash
ssh <SSH_USERNAME>@<APP_SERVER_2_IP>
```

Then:

```bash
ls -l /tmp/file.txt
```

To verify that the file was empty:

```bash
stat -c %s /tmp/file.txt
```

Expected output:

```text
0
```

This confirms that the required empty file was created successfully.

---

# Understanding the Important Ansible Concepts

## 1. Inventory

An inventory file contains information about the servers managed by Ansible.

Example:

```ini
stapp02 ansible_host=<IP> ansible_user=<USERNAME> ansible_password=<PASSWORD>
```

## 2. Playbook

An Ansible playbook is a YAML file that contains instructions for automating tasks on remote servers.

Example:

```yaml
---
- name: Create file
  hosts: stapp02
  tasks:
    - name: Create an empty file
      file:
        path: /tmp/file.txt
        state: touch
```

## 3. File Module

The Ansible `file` module manages files, directories, permissions, and ownership.

For this challenge, it was used to create an empty file.

## 4. State Touch

```yaml
state: touch
```

This ensures that the file exists. If the file does not exist, Ansible creates it.

---

# Troubleshooting and Important Considerations

During this challenge, the existing Ansible configuration was incomplete.

The following points were important:

- Make sure the inventory targets `stapp02`, not the previously configured server.
- Use the correct IP address of App Server 2.
- Configure the SSH username and password inside the inventory.
- Make sure the inventory hostname matches the `hosts` value in the playbook.
- Check YAML indentation carefully.
- Execute the playbook from the correct working directory.
- Use the exact validation command required by the challenge.

### Important Validation Requirement

The task specifically required running:

```bash
ansible-playbook -i inventory playbook.yml
```

Therefore, the connection details had to be available inside the inventory file.

---

# What I Learned

From this challenge, I learned and practiced:

- How Ansible inventory files work.
- How to configure remote server connection details.
- How to update an existing inventory file.
- How to target a specific server using its inventory hostname.
- How to create Ansible playbooks using YAML.
- How to use the Ansible `file` module.
- How `state: touch` creates an empty file.
- How to execute playbooks using `ansible-playbook`.
- Why inventory and playbook hostnames must match.
- How Ansible automates repetitive server administration tasks.
- Why connection details should be configured properly before executing a playbook.

# Challenge Status

Day 83 — Completed Successfully ✅

**Configured the Ansible inventory for App Server 2 and successfully automated empty file creation using an Ansible playbook.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
