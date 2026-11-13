# Day 75 - Commands

This file contains the commands used during **Day 75 of my 100 Days of DevOps Challenge by KodeKloud**.

The objective was to configure three application servers as Jenkins SSH build agents and make sure all agents successfully connected to the Jenkins controller.

---

# Step 1: Access App_server_1

Connect to the first application server:

```bash
ssh <username>@<app-server-1>
```

Verify the server:

```bash
hostname
```

---

# Step 2: Install Java 17 on App_server_1

Install Java 17:

```bash
sudo yum install java-17-openjdk java-17-openjdk-devel -y
```

Verify Java:

```bash
java -version
```

Check the Java executable location:

```bash
which java
```

---

# Step 3: Access App_server_2

Connect to the second application server:

```bash
ssh <username>@<app-server-2>
```

Verify the server:

```bash
hostname
```

---

# Step 4: Install Java 17 on App_server_2

Install Java 17:

```bash
sudo yum install java-17-openjdk java-17-openjdk-devel -y
```

Verify Java:

```bash
java -version
```

Check Java location:

```bash
which java
```

---

# Step 5: Access App_server_3

Connect to the third application server:

```bash
ssh <username>@<app-server-3>
```

Verify the server:

```bash
hostname
```

---

# Step 6: Install Java 17 on App_server_3

Install Java 17:

```bash
sudo yum install java-17-openjdk java-17-openjdk-devel -y
```

Verify Java:

```bash
java -version
```

Check Java location:

```bash
which java
```

---

# Step 7: Verify Java Installation on All Servers

Run the following command on each application server:

```bash
java -version
```

Expected output should indicate Java 17.

Example:

```text
openjdk version "17.x.x"
```

---

# Step 8: Jenkins Dashboard Configuration

The following steps are performed through the Jenkins web interface rather than the terminal.

### Install SSH Build Agents Plugin

```text
Manage Jenkins
      ↓
Plugins
      ↓
Available plugins
      ↓
Search: SSH Build Agents
      ↓
Install
```

### Add Jenkins Agents

Navigate to:

```text
Manage Jenkins
      ↓
Nodes
      ↓
New Node
      ↓
Enter Node Name
      ↓
Permanent Agent
      ↓
Configure Settings
      ↓
Save
```

---

# Step 9: Configure App_server_1

Node configuration:

```text
Node Name: App_server_1
Number of Executors: 1
Remote Root Directory: <remote-root-directory>
Labels: <app-server-1-label>
Launch Method: Launch agents via SSH
Host: <app-server-1-hostname>
Credentials: <app-server-1-ssh-credentials>
```

Save the node configuration.

---

# Step 10: Configure App_server_2

Node configuration:

```text
Node Name: App_server_2
Number of Executors: 1
Remote Root Directory: <remote-root-directory>
Labels: <app-server-2-label>
Launch Method: Launch agents via SSH
Host: <app-server-2-hostname>
Credentials: <app-server-2-ssh-credentials>
```

Save the node configuration.

---

# Step 11: Configure App_server_3

Node configuration:

```text
Node Name: App_server_3
Number of Executors: 1
Remote Root Directory: <remote-root-directory>
Labels: <app-server-3-label>
Launch Method: Launch agents via SSH
Host: <app-server-3-hostname>
Credentials: <app-server-3-ssh-credentials>
```

Save the node configuration.

---

# Step 12: Troubleshoot Java Compatibility

Initially, the agents failed to connect because of Java compatibility issues.

To resolve the problem, I installed Java 17 on all three application servers.

Installation command:

```bash
sudo yum install java-17-openjdk java-17-openjdk-devel -y
```

Verify the Java version:

```bash
java -version
```

After installing Java 17, I checked the Jenkins Nodes dashboard again.

---

# Step 13: Verify Jenkins Agents

Open the Jenkins dashboard:

```text
Manage Jenkins → Nodes
```

Check the status of all three agents.

Expected result:

```text
App_server_1    Online
App_server_2    Online
App_server_3    Online
```

---

# Quick Command Summary

```bash
# App_server_1
ssh <username>@<app-server-1>
hostname
sudo yum install java-17-openjdk java-17-openjdk-devel -y
java -version
which java

# App_server_2
ssh <username>@<app-server-2>
hostname
sudo yum install java-17-openjdk java-17-openjdk-devel -y
java -version
which java

# App_server_3
ssh <username>@<app-server-3>
hostname
sudo yum install java-17-openjdk java-17-openjdk-devel -y
java -version
which java
```

---

# Important Jenkins Configuration

The following settings were configured through the Jenkins dashboard:

```text
Plugin: SSH Build Agents

Agent Type: Permanent Agent

Launch Method: Launch agents via SSH

Agents:
- App_server_1
- App_server_2
- App_server_3

Java Version: 17

Final Agent Status: Online
```

---

# Important Lesson

Jenkins agents need a compatible Java runtime to execute their agent processes.

During this challenge, the main issue was an incompatible Java version on the application servers.

Installing Java 17 on all three servers resolved the problem.

The most important installation command was:

```bash
sudo yum install java-17-openjdk java-17-openjdk-devel -y
```

And the verification command was:

```bash
java -version
```

**Day 75 completed successfully. All three Jenkins SSH Build Agents were connected and Online.** 🚀
