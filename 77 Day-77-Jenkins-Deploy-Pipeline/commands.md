# Day 77 - Commands

This file contains the commands and configuration used during **Day 77 of my 100 Days of DevOps Challenge**.

The objective was to configure a Jenkins agent on App Server 1 and create a deployment pipeline that pulls the latest website code from Gitea into `/var/www/html`.

---

# Step 1: Access App Server 1

Connect to App Server 1 using SSH:

```bash
ssh sarah@stapp01
```

Use the credentials provided by the lab.

---

# Step 2: Verify the Server

Check the hostname:

```bash
hostname
```

Check the current user:

```bash
whoami
```

Expected user:

```text
sarah
```

---

# Step 3: Check Java Installation

Jenkins agents require Java to run.

Check the Java version:

```bash
java -version
```

If Java is not installed, install the appropriate Java package supported by the Jenkins environment:

```bash
sudo yum install java-17-openjdk -y
```

Verify again:

```bash
java -version
```

---

# Step 4: Prepare the Jenkins Agent Directory

The required remote root directory is:

```text
/home/sarah/jenkins_agent
```

Create the directory if it does not already exist:

```bash
mkdir -p /home/sarah/jenkins_agent
```

Verify:

```bash
ls -ld /home/sarah/jenkins_agent
```

The directory should be accessible to the `sarah` user.

---

# Step 5: Verify the Existing Website Repository

Move to the Apache document root:

```bash
cd /var/www/html
```

List the files:

```bash
ls -la
```

Check Git repository status:

```bash
git status
```

Check the configured remote repository:

```bash
git remote -v
```

This confirms that the existing repository is connected to Gitea.

---

# Step 6: Verify Apache Document Root

The deployment directory is:

```text
/var/www/html
```

Check its contents:

```bash
ls -la /var/www/html
```

The website files should be present directly inside this directory.

---

# Step 7: Update the Repository Manually (Optional Verification)

Move into the repository:

```bash
cd /var/www/html
```

Pull the latest code:

```bash
git pull origin master
```

This is the same Git operation that will be executed by the Jenkins pipeline.

Check the repository status:

```bash
git status
```

---

# Step 8: Configure Jenkins Agent

This configuration is performed through the Jenkins web interface, not the Linux terminal.

Navigate to:

```text
Jenkins Dashboard
    → Manage Jenkins
    → Nodes
    → New Node
```

Configure:

```text
Node Name: App Server 1
Type: Permanent Agent
Remote Root Directory: /home/sarah/jenkins_agent
Labels: stapp01
Launch Method: Launch agents via SSH
Host: stapp01
SSH User: sarah
```

Select the provided SSH credentials and save the node.

Verify that the agent is online.

---

# Step 9: Create Jenkins Pipeline Job

From the Jenkins Dashboard:

1. Click New Item.
2. Enter the job name.
3. Select Pipeline.
4. Do not select Multibranch Pipeline.
5. Click OK.

Required job name:

```text
xfusion-webapp-job
```

---

# Step 10: Jenkins Pipeline Script

Go to the Pipeline section and select:

```text
Definition: Pipeline script
```

Paste the following code:

```groovy
pipeline {
    agent {
        label 'stapp01'
    }

    stages {
        stage('Deploy') {
            steps {
                script {
                    sh '''
                        cd /var/www/html
                        git pull origin master
                    '''
                }
            }
        }
    }
}
```

Save the pipeline.

---

# Step 11: Execute the Pipeline

From the Jenkins dashboard:

1. Open `xfusion-webapp-job`.
2. Click Build Now.
3. Open Console Output.
4. Check the execution logs.

Expected important output:

```text
Running on App Server 1

[Pipeline] stage
[Pipeline] { (Deploy)

+ cd /var/www/html
+ git pull origin master

Already up to date.

Finished: SUCCESS
```

The exact Git output may differ depending on whether new changes are available.

---

# Step 12: Verify Deployment

Check the website files:

```bash
ls -la /var/www/html
```

Check the Apache service:

```bash
sudo systemctl status httpd
```

Check whether Apache is listening on port 8080:

```bash
sudo ss -tlnp | grep 8080
```

If curl is available, test the local website:

```bash
curl http://localhost:8080/
```

Finally, open the KodeKloud **App** button to verify the website through the load balancer.

The website should load from:

```text
https://<load-balancer-url>/
```

---

# Quick Command Summary

```bash
# Connect to App Server 1
ssh sarah@stapp01

# Verify server and user
hostname
whoami

# Check Java
java -version

# Install Java if required
sudo yum install java-17-openjdk -y

# Create Jenkins agent directory
mkdir -p /home/sarah/jenkins_agent

# Verify agent directory
ls -ld /home/sarah/jenkins_agent

# Navigate to website repository
cd /var/www/html

# Verify repository
git status

# Check remote repository
git remote -v

# Pull latest changes
git pull origin master

# Verify website files
ls -la /var/www/html

# Check Apache service
sudo systemctl status httpd

# Check Apache port
sudo ss -tlnp | grep 8080

# Test website locally
curl http://localhost:8080/
```

---

# Jenkins Pipeline Script

The most important configuration for this challenge was the following pipeline:

```groovy
pipeline {
    agent {
        label 'stapp01'
    }

    stages {
        stage('Deploy') {
            steps {
                script {
                    sh '''
                        cd /var/www/html
                        git pull origin master
                    '''
                }
            }
        }
    }
}
```

---

# Understanding the Important Commands

## 1. git status

```bash
git status
```

Shows the current state of the Git repository.

## 2. git remote -v

```bash
git remote -v
```

Displays the remote repository URL.

## 3. git pull origin master

```bash
git pull origin master
```

Fetches and integrates the latest changes from the remote `master` branch.

## 4. systemctl status httpd

```bash
sudo systemctl status httpd
```

Checks whether the Apache web server is running.

## 5. curl

```bash
curl http://localhost:8080/
```

Tests whether the website responds locally on port 8080.

---

# Important Lesson

For Jenkins deployment tasks, always understand where the source code is located and where the application needs to be deployed.

In this challenge, the repository was already cloned inside:

```text
/var/www/html
```

Therefore, the correct deployment operation was:

```bash
cd /var/www/html
git pull origin master
```

The Jenkins agent executes this command remotely on App Server 1.

The most important concepts from this challenge were:

- Jenkins Agents
- Jenkins Pipeline
- Gitea Integration
- Git Pull
- Automated Deployment
- Apache Document Root

**Day 77 completed successfully.** 🚀
