# Day 81 - Commands

This file contains the commands used during **Day 81 of my 100 Days of DevOps Challenge**.

The objective was to deploy a static website from a Gitea repository to App Server 1 using a Jenkins Pipeline.

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

# Step 2: Navigate to Website Repository

Move into the website repository:

```bash
cd /var/www/html
```

Check the repository files:

```bash
ls -la
```

---

# Step 3: Update index.html

Open the website file:

```bash
vi index.html
```

Update its content to:

```html
Welcome to xFusionCorp Industries
```

Save and exit the file.

---

# Step 4: Commit Changes to Git

Check the repository status:

```bash
git status
```

Add the modified file:

```bash
git add index.html
```

Commit the changes:

```bash
git commit -m "Update website welcome message"
```

Push the changes to the master branch:

```bash
git push origin master
```

Verify the latest commit:

```bash
git log -1 --oneline
```

---

# Step 5: Install Java 17

Check whether Java is installed:

```bash
java -version
```

If Java 17 is missing, install it:

```bash
sudo yum install java-17-openjdk -y
```

Verify Java installation:

```bash
java -version
```

Check the Java executable location:

```bash
which java
```

---

# Step 6: Create Jenkins Agent Directory

Create the remote root directory for the Jenkins Agent:

```bash
mkdir -p /home/sarah/jenkins_agent
```

Verify the directory:

```bash
ls -ld /home/sarah/jenkins_agent
```

---

# Step 7: Jenkins Agent Configuration

Configure the following settings from the Jenkins dashboard:

```text
Node Name: App Server 1

Node Type: Permanent Agent

Remote Root Directory:
/home/sarah/jenkins_agent

Label:
stapp01

Launch Method:
Launch agents via SSH

Host:
stapp01

Username:
sarah

Credentials:
SSH credentials for sarah
```

After saving, verify that the Jenkins Agent is online.

---

# Step 8: Create Jenkins Pipeline Job

From the Jenkins dashboard:

```text
New Item
    |
    v
Job Name: deploy-job
    |
    v
Select: Pipeline
    |
    v
Click OK
```

Configure the Pipeline script.

---

# Step 9: Jenkins Pipeline Script

The following is the illustrative pipeline logic used for deployment and testing, assuming the repository is already cloned at `/var/www/html` on App Server 1.

```groovy
node('stapp01') {

    stage('Deploy') {

        sh '''
            cd /var/www/html
            git pull origin master
        '''
    }

    stage('Test') {

        sh '''
            curl --fail http://stlb01:8091
        '''
    }
}
```

### Pipeline Explanation

**Node:**

```groovy
node('stapp01')
```

Runs the pipeline on the Jenkins Agent with the `stapp01` label.

**Deploy Stage:**

```groovy
stage('Deploy') {
    sh '''
        cd /var/www/html
        git pull origin master
    '''
}
```

Updates the website repository from Gitea.

**Test Stage:**

```groovy
stage('Test') {
    sh '''
        curl --fail http://stlb01:8091
    '''
}
```

Checks whether the website is accessible.

---

# Step 10: Test Website Using curl

Run the following command from a machine that can access the website:

```bash
curl http://stlb01:8091
```

To fail on HTTP errors:

```bash
curl --fail http://stlb01:8091
```

To check the exact welcome message:

```bash
curl --fail --silent http://stlb01:8091 | grep -F "Welcome to xFusionCorp Industries"
```

Expected website content:

```text
Welcome to xFusionCorp Industries
```

---

# Step 11: Verify Apache Service

Apache was already installed and running for this challenge, so no additional setup was required.

To inspect its status if needed:

```bash
sudo systemctl status httpd
```

Check listening ports:

```bash
sudo ss -tulnp | grep 8080
```

---

# Step 12: Verify Git Repository

Check the current branch:

```bash
git branch
```

Check repository status:

```bash
git status
```

Check the latest commit:

```bash
git log -1 --oneline
```

---

# Quick Command Summary

```bash
# Connect to App Server 1
ssh sarah@stapp01

# Verify server
hostname

# Navigate to website repository
cd /var/www/html

# Update website
vi index.html

# Commit changes
git add index.html
git commit -m "Update website welcome message"

# Push changes to Gitea
git push origin master

# Verify Java
java -version

# Install Java 17 if required
sudo yum install java-17-openjdk -y

# Create Jenkins Agent directory
mkdir -p /home/sarah/jenkins_agent

# Verify Jenkins Agent directory
ls -ld /home/sarah/jenkins_agent

# Verify Git repository
git status

# Verify latest commit
git log -1 --oneline

# Check Apache status
sudo systemctl status httpd

# Check Apache listening port
sudo ss -tulnp | grep 8080

# Test website
curl --fail http://stlb01:8091

# Verify exact website content
curl --fail --silent http://stlb01:8091 | grep -F "Welcome to xFusionCorp Industries"
```

---

# Important Jenkins Concepts

## Jenkins Controller

The main Jenkins server that manages jobs, pipelines, and agents.

## Jenkins Agent

A remote machine that executes pipeline tasks.

## Jenkins Label

A label identifies which agent should execute a particular pipeline.

Example:

```groovy
node('stapp01')
```

## Jenkins Pipeline

A series of automated stages used to build, deploy, and test applications.

## Deploy Stage

Updates the website files in:

```text
/var/www/html
```

## Test Stage

Checks website availability using:

```bash
curl --fail http://stlb01:8091
```

---

# Important Lesson

This challenge helped me understand how Jenkins can automate website deployment using remote agents and pipelines.

Instead of manually deploying website files, Jenkins performs the deployment and testing through defined pipeline stages.

The most important pipeline configuration was:

```groovy
node('stapp01')
```

The deployment directory was:

```text
/var/www/html
```

The final website URL was:

```text
http://stlb01:8091
```

The successful Jenkins build result was:

```text
Finished: SUCCESS
```

**Day 81 completed successfully.** 🚀
