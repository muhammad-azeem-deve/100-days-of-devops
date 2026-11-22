# Day 78 - Commands

This file contains the commands used during **Day 78 of my 100 Days of DevOps Challenge**.

The objective was to configure App Server 1 as a Jenkins Agent and create a conditional Jenkins Pipeline to deploy the `master` and `feature` branches of the website.

---

# Step 1: Access App Server 1

Connect to App Server 1:

```bash
ssh sarah@stapp01
```

Verify the server:

```bash
hostname
```

---

# Step 2: Install Java

Install Java:

```bash
sudo yum install java-11-openjdk -y
```

Verify Java:

```bash
java -version
```

---

# Step 3: Create Jenkins Agent Directory

Create the Jenkins Agent working directory:

```bash
sudo mkdir -p /home/sarah/jenkins_agent
```

Set ownership:

```bash
sudo chown -R sarah:sarah /home/sarah/jenkins_agent
```

Verify the directory:

```bash
ls -ld /home/sarah/jenkins_agent
```

---

# Step 4: Configure Website Directory Permissions

Change ownership of the website directory:

```bash
sudo chown -R sarah:sarah /var/www/html
```

Verify ownership:

```bash
ls -ld /var/www/html
```

---

# Step 5: Verify Java Installation

```bash
java -version
```

Check Java location:

```bash
which java
```

---

# Step 6: Jenkins Plugin Installation

Install the required plugins through the Jenkins dashboard.

Navigate to:

```text
Manage Jenkins → Plugins
```

Required plugins:

```text
Pipeline
Git
SSH Build Agents
```

---

# Step 7: Configure Jenkins Agent

Configure the following details in Jenkins:

```text
Node Name: App Server 1

Remote Root Directory:
/home/sarah/jenkins_agent

Labels:
stapp01

Launch Method:
Launch agents via SSH

SSH Username:
sarah
```

Save the configuration and verify that the agent is online.

---

# Step 8: Create Jenkins Pipeline Job

Open Jenkins Dashboard.

Create a new item:

```text
nautilus-webapp-job
```

Select:

```text
Pipeline
```

Do not select:

```text
Multibranch Pipeline
```

---

# Step 9: Configure String Parameter

Enable parameterization and add:

```text
Name: BRANCH

Default Value: master

Description: Git branch to deploy
```

---

# Step 10: Configure Pipeline Script

Use the following illustrative Pipeline script.

Replace `REPOSITORY_URL` with the actual repository URL from the challenge.

```groovy
pipeline {
    agent {
        label 'stapp01'
    }

    parameters {
        string(
            name: 'BRANCH',
            defaultValue: 'master',
            description: 'Git branch to deploy'
        )
    }

    stages {
        stage('Deploy') {
            steps {
                script {
                    if (params.BRANCH == 'master') {
                        git branch: 'master',
                            url: 'REPOSITORY_URL'
                    }
                    else if (params.BRANCH == 'feature') {
                        git branch: 'feature',
                            url: 'REPOSITORY_URL'
                    }
                    else {
                        error("Invalid branch selected")
                    }
                }

                sh '''
                    rsync -av --delete --exclude='.git' ./ /var/www/html/
                '''
            }
        }
    }
}
```

---

# Step 11: Verify Website Directory

Check the website directory:

```bash
ls -l /var/www/html
```

Verify directory ownership:

```bash
ls -ld /var/www/html
```

Expected owner:

```text
sarah
```

---

# Step 12: Test Master Branch Deployment

In Jenkins, click:

```text
Build with Parameters
```

Select:

```text
BRANCH = master
```

Run the build.

After successful deployment, open the KodeKloud App button.

Expected website content:

```text
Welcome to xFusionCorp Industries!
```

---

# Step 13: Test Feature Branch Deployment

Run the same Pipeline job again.

Select:

```text
BRANCH = feature
```

After the build completes, open the website.

Expected website content:

```text
Updated
```

---

# Step 14: Restore Master Branch

Run the pipeline again:

```text
BRANCH = master
```

Expected website content:

```text
Welcome to xFusionCorp Industries!
```

---

# Quick Command Summary

```bash
# Connect to App Server 1
ssh sarah@stapp01

# Verify server
hostname

# Install Java
sudo yum install java-11-openjdk -y

# Verify Java
java -version

# Create Jenkins Agent directory
sudo mkdir -p /home/sarah/jenkins_agent

# Set Jenkins Agent directory ownership
sudo chown -R sarah:sarah /home/sarah/jenkins_agent

# Set website directory ownership
sudo chown -R sarah:sarah /var/www/html

# Verify Agent directory
ls -ld /home/sarah/jenkins_agent

# Verify website directory
ls -ld /var/www/html

# Check Java location
which java

# Check website files
ls -l /var/www/html
```

---

# Important Jenkins Configuration

The following configuration values were important for this challenge:

```text
Jenkins Job:
nautilus-webapp-job

Job Type:
Pipeline

Agent:
App Server 1

Agent Label:
stapp01

Agent User:
sarah

Agent Working Directory:
/home/sarah/jenkins_agent

Deployment Directory:
/var/www/html

Pipeline Parameter:
BRANCH

Available Branches:
master
feature

Pipeline Stage:
Deploy
```

---

# Understanding Conditional Pipeline

The most important part of this challenge was the conditional logic.

```groovy
if (params.BRANCH == 'master') {
    // Deploy master
}
else if (params.BRANCH == 'feature') {
    // Deploy feature
}
```

The pipeline checks the selected parameter and decides which Git branch should be deployed.

---

# Final Verification

The final deployment results were:

```text
Build 1:
BRANCH = master

Website:
Welcome to xFusionCorp Industries!
```

```text
Build 2:
BRANCH = feature

Website:
Updated
```

```text
Build 3:
BRANCH = master

Website:
Welcome to xFusionCorp Industries!
```

All three builds were tested successfully through the KodeKloud App button.

---

# Important Lesson

This challenge helped me understand how Jenkins can automate deployment based on user-selected parameters.

Instead of creating separate Jenkins jobs for different branches, I used one Pipeline job with conditional logic.

The most important concepts were:

```text
Jenkins Agent
Git Branch
Pipeline
String Parameter
Conditional Statements
Automated Deployment
```

**Day 78 completed successfully.** 🚀
