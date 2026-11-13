# Day 75 - Jenkins Slave Nodes (SSH Build Agents)

## Challenge Overview

This is Day 75 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Jenkins Slave Nodes (SSH Build Agents)**. The task required configuring three application servers (`App_server_1`, `App_server_2`, and `App_server_3`) as SSH build agents in Jenkins.

The objective was to connect all three application servers to the Jenkins controller using SSH, configure their node settings, and make sure all agents were successfully connected and showing **Online** status.

During this challenge, I initially faced connection failures because of an incompatible Java version on the application servers. I resolved this issue by installing Java 17 on all three servers.

### Challenge Requirement

Configure the following three application servers as Jenkins SSH build agents:

- App_server_1
- App_server_2
- App_server_3

The task required:

1. Install the SSH Build Agents plugin in Jenkins.
2. Install Java 17 on all three application servers.
3. Configure SSH access and credentials.
4. Add all three application servers as permanent Jenkins agents.
5. Configure the required labels and remote root directories.
6. Connect the agents using SSH.
7. Verify that all three agents show Online status.

---

## Objectives

The objectives of this challenge are:

- Understand Jenkins controller and agent architecture.
- Install the SSH Build Agents plugin.
- Install Java 17 on all application servers.
- Configure SSH connectivity between Jenkins and application servers.
- Add application servers as permanent Jenkins nodes.
- Configure node names, executors, labels, and remote root directories.
- Configure SSH credentials for each agent.
- Troubleshoot Java compatibility issues.
- Verify that all three Jenkins agents are online.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 75 |
| CI/CD Tool | Jenkins |
| Plugin | SSH Build Agents |
| Agent 1 | App_server_1 |
| Agent 2 | App_server_2 |
| Agent 3 | App_server_3 |
| Connection Method | SSH |
| Java Version | Java 17 |
| Agent Type | Permanent Agent |
| Final Status | Online |
| Status | Completed |

---

# Solution

## Step 1: Understand Jenkins Controller and Agents

Before starting the configuration, I understood the role of Jenkins controller and agents.

**Jenkins Controller:**

The controller manages Jenkins jobs, pipelines, configurations, and assigns tasks to agents.

**Jenkins Agents:**

Agents are remote machines that execute the build tasks assigned by the Jenkins controller.

In this challenge, I configured three application servers as Jenkins agents.

```text
                Jenkins Controller
                       |
                 SSH Connection
                       |
          +------------+------------+
          |            |            |
          v            v            v
      App_server_1 App_server_2 App_server_3
          |            |            |
          v            v            v
        Agent 1      Agent 2      Agent 3
          |            |            |
        Online       Online       Online
```

---

## Step 2: Install Java 17 on App_server_1

First, I accessed App_server_1 using SSH.

```bash
ssh <username>@<app-server-1>
```

I installed Java 17 using:

```bash
sudo yum install java-17-openjdk java-17-openjdk-devel -y
```

After installation, I verified the Java version:

```bash
java -version
```

This confirmed that Java 17 was installed successfully.

---

## Step 3: Install Java 17 on App_server_2

I connected to App_server_2:

```bash
ssh <username>@<app-server-2>
```

Then I installed Java 17:

```bash
sudo yum install java-17-openjdk java-17-openjdk-devel -y
```

I verified the installation:

```bash
java -version
```

---

## Step 4: Install Java 17 on App_server_3

I followed the same process on App_server_3.

Connect to the server:

```bash
ssh <username>@<app-server-3>
```

Install Java 17:

```bash
sudo yum install java-17-openjdk java-17-openjdk-devel -y
```

Verify:

```bash
java -version
```

After completing these steps, Java 17 was available on all three application servers.

---

## Step 5: Install SSH Build Agents Plugin

After preparing the application servers, I accessed the Jenkins dashboard.

I followed these steps:

1. Open Jenkins in the browser.
2. Navigate to **Manage Jenkins**.
3. Open **Plugins**.
4. Select **Available plugins**.
5. Search for `SSH Build Agents`.
6. Select the plugin.
7. Click Install.

The SSH Build Agents plugin allows Jenkins to connect to remote machines using SSH.

---

## Step 6: Configure App_server_1 as a Jenkins Agent

After installing the plugin, I navigated to:

**Manage Jenkins → Nodes → New Node**

I entered the node name:

```text
App_server_1
```

Then I selected:

```text
Permanent Agent
```

I configured the following settings:

- **Node Name:** App_server_1
- **Number of Executors:** 1
- **Remote Root Directory:** The required directory on the application server
- **Labels:** The required label for App_server_1
- **Usage:** Use this node as much as possible
- **Launch Method:** Launch agents via SSH
- **Host:** App_server_1 hostname
- **Credentials:** SSH credentials for App_server_1

After entering the required information, I saved the configuration.

---

## Step 7: Configure App_server_2 as a Jenkins Agent

I repeated the same process for App_server_2.

I navigated to:

**Manage Jenkins → Nodes → New Node**

I entered:

```text
App_server_2
```

I selected Permanent Agent and configured:

- Node Name
- Number of Executors
- Remote Root Directory
- Labels
- SSH Launch Method
- App_server_2 Hostname
- SSH Credentials

After configuring all required settings, I saved the node.

---

## Step 8: Configure App_server_3 as a Jenkins Agent

I repeated the configuration process for App_server_3.

I entered:

```text
App_server_3
```

I configured the node as a Permanent Agent and provided its hostname, SSH credentials, labels, executors, and remote root directory.

Finally, I saved the configuration.

---

## Step 9: Troubleshooting Java Compatibility Issue

Initially, all three agents faced connection problems because of an incompatible Java version on the application servers.

Jenkins requires a compatible Java runtime to execute the agent process.

To resolve this issue, I installed Java 17 on all three application servers.

The installation command was:

```bash
sudo yum install java-17-openjdk java-17-openjdk-devel -y
```

After installing Java 17, I verified it:

```bash
java -version
```

Then I returned to the Jenkins Nodes dashboard and checked the agent connections again.

This resolved the Java compatibility issue, and the agents successfully connected.

---

## Step 10: Verify All Jenkins Agents

Finally, I opened:

**Manage Jenkins → Nodes**

I checked the connection status of all three application servers.

Expected result:

| Jenkins Agent | Status |
|---|---|
| App_server_1 | Online |
| App_server_2 | Online |
| App_server_3 | Online |

All three application servers successfully connected to the Jenkins controller through SSH and displayed **Online** status.

---

# Troubleshooting

### Problem: Jenkins Agents Failed to Connect

Initially, the Jenkins agents could not connect successfully.

**Cause:**

The application servers had an incompatible Java version.

**Solution:**

I installed Java 17 on all three application servers.

```bash
sudo yum install java-17-openjdk java-17-openjdk-devel -y
```

Then I verified the Java installation:

```bash
java -version
```

After fixing the Java compatibility issue, all three agents connected successfully.

### Other Important Checks

If an SSH agent still fails to connect, check:

- SSH connectivity between Jenkins and the application server.
- Correct hostname or IP address.
- Correct SSH username and credentials.
- SSH service availability.
- Remote root directory permissions.
- Java version compatibility.
- Jenkins agent connection logs.

---

# What I Learned

From this challenge, I learned and practiced:

- How Jenkins controller and agents work.
- The difference between Jenkins controller and build agents.
- How to install Jenkins plugins.
- How to configure SSH Build Agents.
- How to install Java 17 on Linux application servers.
- How to configure remote servers as permanent Jenkins agents.
- How to configure node names, labels, and executors.
- How to configure remote root directories.
- How Jenkins uses SSH credentials to connect to remote servers.
- How Java compatibility affects Jenkins agent connectivity.
- How to troubleshoot failed agent connections.
- How to verify agent connectivity using the Jenkins Nodes dashboard.

# Challenge Status

Day 75 — Completed Successfully ✅

**Successfully configured three application servers as Jenkins SSH Build Agents. Installed Java 17, resolved the compatibility issue, and verified that all three agents were Online.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
