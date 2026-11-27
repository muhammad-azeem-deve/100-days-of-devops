# Day 82 - Create Ansible Inventory for App Server Testing

## Challenge Overview

This is Day 82 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Ansible Inventory Management**. The Nautilus DevOps team had Ansible playbooks stored in `/home/thor/playbook/` on the Jump Host and wanted to execute them on Application Server 3 in Stratos DC.

To complete the task, I created an INI-format inventory file containing the hostname, IP address, SSH username, and password required to connect to App Server 3.

### Challenge Requirement

Create an Ansible inventory file named `inventory` at:

```text
/home/thor/playbook/inventory
```

The inventory file must contain the connection information for **App Server 3 (`stapp03`)**.

The file must include:

- App Server 3 hostname (`stapp03`).
- Server IP address.
- SSH username.
- SSH password.

All connection information must be available inside the inventory file so that the playbook can execute using:

```bash
ansible-playbook -i inventory playbook.yml
```

---

## Objectives

The objectives of this challenge are:

- Access the Jump Host using the `thor` user.
- Navigate to `/home/thor/playbook/`.
- Understand the purpose of an Ansible inventory.
- Create an INI-format inventory file.
- Add App Server 3 (`stapp03`) to the inventory.
- Configure the server IP address using `ansible_host`.
- Configure SSH authentication using `ansible_user` and `ansible_password`.
- Verify the inventory file.
- Execute the Ansible playbook using the inventory.
- Confirm successful execution against App Server 3.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 82 |
| User | `thor` |
| Working Server | Jump Host |
| Target Server | App Server 3 |
| Target Hostname | `stapp03` |
| Inventory Format | INI |
| Inventory File | `inventory` |
| Inventory Path | `/home/thor/playbook/inventory` |
| Playbook | `playbook.yml` |
| Status | Completed |

---

# Solution

## Step 1: Access the Jump Host

First, I accessed the Jump Host using the `thor` user.

I verified the current user:

```bash
whoami
```

Expected output:

```text
thor
```

---

## Step 2: Navigate to the Playbook Directory

The Ansible playbooks were already available in:

```text
/home/thor/playbook/
```

I navigated to the directory:

```bash
cd /home/thor/playbook/
```

Then I checked its contents:

```bash
ls -l
```

---

## Step 3: Create the Inventory File

Next, I created an inventory file named `inventory` using the `vi` editor:

```bash
vi inventory
```

This created the inventory file in the required directory.

---

## Step 4: Configure the INI Inventory

Inside the inventory file, I added the App Server 3 details.

The inventory structure was:

```ini
[app_servers]
stapp03 ansible_host=<SERVER_IP> ansible_user=<USERNAME> ansible_password=<PASSWORD>
```

### Explanation of Inventory Variables

- `[app_servers]`: Defines a group for application servers.
- `stapp03`: The hostname of Application Server 3.
- `ansible_host`: Specifies the IP address of the target server.
- `ansible_user`: Specifies the SSH username.
- `ansible_password`: Specifies the SSH password.

I replaced the placeholders with the actual connection details provided in the KodeKloud lab.

After entering the information, I saved the file and exited the editor.

---

## Step 5: Verify the Inventory File

After creating the inventory, I verified its contents:

```bash
cat inventory
```

I checked that the file contained:

- The correct hostname (`stapp03`).
- The correct server IP address.
- The correct SSH username.
- The correct SSH password.

This ensured that all required connection details were stored inside the inventory file.

---

## Step 6: Execute the Ansible Playbook

After configuring the inventory, I executed the Ansible playbook using:

```bash
ansible-playbook -i inventory playbook.yml
```

### Explanation of the Command

- `ansible-playbook`: Executes an Ansible playbook.
- `-i`: Specifies the inventory file.
- `inventory`: The inventory file containing the target server details.
- `playbook.yml`: The Ansible playbook to execute.

Because all connection information was already configured inside the inventory file, I did not need to provide additional connection arguments.

---

## Step 7: Verify Successful Execution

After running the playbook, I checked the execution output.

The playbook successfully executed against:

```text
stapp03
```

The execution completed without failures.

This confirmed that:

- The inventory file was created correctly.
- The target server was identified successfully.
- SSH authentication worked using the inventory credentials.
- Ansible successfully connected to App Server 3.
- The playbook executed without failures.

---

# Understanding Ansible Inventory

An Ansible inventory is a file that contains information about the managed servers Ansible needs to communicate with.

It can contain:

- Hostnames.
- IP addresses.
- SSH usernames.
- SSH passwords.
- Groups of servers.
- Connection-related variables.

For this challenge, the INI-format inventory was:

```ini
[app_servers]
stapp03 ansible_host=<SERVER_IP> ansible_user=<USERNAME> ansible_password=<PASSWORD>
```

---

# What I Learned

From this challenge, I learned and practiced:

- What an Ansible inventory is and why it is required.
- How to create an INI-format inventory file.
- How to organize managed servers into inventory groups.
- How to configure `ansible_host`.
- How to configure `ansible_user`.
- How to configure `ansible_password`.
- How Ansible uses inventory variables for SSH authentication.
- How to execute playbooks using the `-i` option.
- How to verify inventory file contents.
- How to execute an Ansible playbook against a specific remote server.
- The importance of keeping all required connection details inside the inventory file.

# Challenge Status

Day 82 — Completed Successfully ✅

**Created an INI-format Ansible inventory and successfully executed the playbook against App Server 3 (`stapp03`) without failures.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
