# Day 81 - Deploy Static Website Using Jenkins Pipeline

## Challenge Overview

This is Day 81 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Jenkins CI/CD Pipeline and Automated Website Deployment**. The objective was to deploy a static website from a Gitea repository to a Nautilus Application Server using a Jenkins Pipeline.

The task involved updating the website content in the Gitea repository, configuring App Server 1 as a Jenkins Agent, creating a Pipeline job, and deploying and testing the website automatically.

### Challenge Requirement

Deploy a static website to the Nautilus App Server using Jenkins.

The task required:

1. Update `index.html` in the `sarah/web` Gitea repository.
2. Change the website content to `Welcome to xFusionCorp Industries`.
3. Push the updated code to the `master` branch.
4. Configure App Server 1 as a Jenkins Agent.
5. Create a Jenkins Agent named `App Server 1` with the label `stapp01`.
6. Configure a Pipeline job named `deploy-job`.
7. Create two pipeline stages: `Deploy` and `Test`.
8. Deploy the website to `/var/www/html`.
9. Test website availability using `curl`.
10. Make sure the website is accessible at `http://stlb01:8091`.

---

## Objectives

The objectives of this challenge are:

- Understand Jenkins CI/CD pipelines.
- Update website content in a Gitea repository.
- Use Git commands to commit and push changes.
- Configure a remote Jenkins Agent.
- Install Java 17 on App Server 1 if required.
- Configure SSH-based communication between Jenkins Controller and Agent.
- Understand Jenkins nodes and labels.
- Create a regular Jenkins Pipeline job.
- Configure Deploy and Test stages.
- Use the `stapp01` label to execute the pipeline on App Server 1.
- Deploy website files to the Apache document root.
- Test the website using `curl`.
- Verify the successful deployment through the browser.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 81 |
| Repository | `sarah/web` |
| Git Branch | `master` |
| Website File | `index.html` |
| Application Server | App Server 1 |
| Jenkins Agent | `App Server 1` |
| Agent Label | `stapp01` |
| Agent User | `sarah` |
| Remote Root | `/home/sarah/jenkins_agent` |
| Java Version | Java 17 |
| Web Server | Apache |
| Web Root | `/var/www/html` |
| Jenkins Job | `deploy-job` |
| Pipeline Stages | Deploy, Test |
| Website URL | `http://stlb01:8091` |
| Status | Completed |

---

# Solution

## Step 1: Access App Server 1

First, I accessed App Server 1 using SSH.

The SSH command format is:

```bash
ssh sarah@stapp01
```

After connecting, I verified the server:

```bash
hostname
```

---

## Step 2: Update the Website Content

The Gitea repository `sarah/web` was already cloned at:

```text
/var/www/html
```

I moved into the repository directory:

```bash
cd /var/www/html
```

I opened the `index.html` file:

```bash
vi index.html
```

I updated the website content to:

```html
Welcome to xFusionCorp Industries
```

Then I saved the file.

---

## Step 3: Commit and Push Changes to Gitea

After updating `index.html`, I added the modified file to Git:

```bash
git add index.html
```

I committed the changes:

```bash
git commit -m "Update website welcome message"
```

Finally, I pushed the changes to the `master` branch:

```bash
git push origin master
```

This updated the Gitea repository with the required website content.

---

## Step 4: Install Java 17 on App Server 1

Before configuring App Server 1 as a Jenkins Agent, I checked whether Java was installed.

```bash
java -version
```

If Java 17 was not installed, I installed it using:

```bash
sudo yum install java-17-openjdk -y
```

After installation, I verified Java again:

```bash
java -version
```

Java is required to run the Jenkins Agent process.

---

## Step 5: Configure App Server 1 as a Jenkins Agent

Next, I opened the Jenkins dashboard and configured App Server 1 as a remote Jenkins Agent.

I navigated to:

```text
Manage Jenkins → Nodes → New Node
```

I created a permanent node with the following configuration:

| Configuration | Value |
|---|---|
| Node Name | `App Server 1` |
| Type | Permanent Agent |
| Number of Executors | 1 |
| Remote Root Directory | `/home/sarah/jenkins_agent` |
| Labels | `stapp01` |
| Usage | Use this node as much as possible |
| Launch Method | Launch agents via SSH |
| Host | `stapp01` |
| Credentials | SSH credentials for `sarah` |

I configured the SSH credentials for the `sarah` user and saved the node configuration.

After saving, I checked the agent status and confirmed that it was online.

---

## Step 6: Create a Jenkins Pipeline Job

After configuring the Jenkins Agent, I created a new Jenkins job.

I followed these steps:

1. Open the Jenkins dashboard.
2. Click **New Item**.
3. Enter the job name:

```text
deploy-job
```

4. Select **Pipeline**.
5. Click OK.

I created a regular Pipeline job instead of a Multibranch Pipeline.

---

## Step 7: Configure the Pipeline Script

The pipeline needed two stages:

- Deploy
- Test

The pipeline was configured to execute on App Server 1 using the `stapp01` label.

The following illustrates the deployment and testing logic:

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

### Understanding the Pipeline

**Node:**

```groovy
node('stapp01')
```

This tells Jenkins to execute the pipeline on the agent with the `stapp01` label.

**Deploy Stage:**

```groovy
stage('Deploy') {
    sh '''
        cd /var/www/html
        git pull origin master
    '''
}
```

This updates the website files from the latest code in the Gitea master branch.

**Test Stage:**

```groovy
stage('Test') {
    sh '''
        curl --fail http://stlb01:8091
    '''
}
```

This checks whether the website is accessible.

The `--fail` option makes the command return a failure status for HTTP error responses, causing the pipeline stage to fail.

---

## Step 8: Save and Execute the Pipeline

After configuring the Pipeline script, I saved the Jenkins job.

Then I opened:

```text
deploy-job
```

I clicked:

```text
Build Now
```

Jenkins started executing the pipeline on App Server 1.

The pipeline executed the following stages:

```text
Deploy
  |
  |-- Update website from Gitea
  |
  v
Test
  |
  |-- Check website using curl
  |
  v
Build Successful
```

---

## Step 9: Verify Jenkins Build

After the pipeline execution, I opened the latest build and checked its Console Output.

The first build completed successfully.

Expected final output:

```text
Finished: SUCCESS
```

The successful build confirmed that the deployment and testing stages completed without errors.

---

## Step 10: Verify the Website

Finally, I opened the website in the browser:

```text
http://stlb01:8091
```

The website displayed the required content:

```text
Welcome to xFusionCorp Industries
```

The website was accessible directly from the root URL without requiring a `/web` subdirectory.

This confirmed that the static website had been deployed successfully.

---

# Jenkins Pipeline Workflow

The complete workflow of this challenge was:

```text
Gitea Repository
      |
      v
Update index.html
      |
      v
Git Commit and Push
      |
      v
Jenkins Controller
      |
      v
Configure Jenkins Agent
      |
      v
App Server 1 (stapp01)
      |
      v
Create deploy-job
      |
      v
Deploy Stage
      |
      v
Update /var/www/html
      |
      v
Test Stage
      |
      v
curl http://stlb01:8091
      |
      v
Finished: SUCCESS
      |
      v
Website Successfully Deployed
```

---

# What I Learned

From this challenge, I learned and practiced:

- How Jenkins CI/CD pipelines automate application deployment.
- How to update website files in a Gitea repository.
- How to use Git commands to commit and push changes.
- How to configure a remote Jenkins Agent.
- How to install and verify Java 17.
- How Jenkins Controller and Agents communicate.
- How to configure SSH-based Jenkins Agent connections.
- How to create permanent Jenkins nodes.
- How Jenkins labels are used to select execution agents.
- How to create a regular Jenkins Pipeline job.
- How to define multiple stages in a Jenkins Pipeline.
- How to deploy website files to an Apache document root.
- How to test website availability using `curl`.
- How to verify a deployment through Jenkins Console Output and a browser.

# Challenge Status

Day 81 — Completed Successfully ✅

**Build #1 — Finished: SUCCESS**

The static website was successfully deployed using a Jenkins Pipeline, and the required welcome message was accessible at the root URL.

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
