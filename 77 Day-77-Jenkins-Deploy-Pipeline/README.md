# Day 77 - Jenkins Deploy Pipeline

## Challenge Overview

This is Day 77 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Jenkins CI/CD Pipeline and Automated Deployment**. The task required configuring a Jenkins agent on App Server 1 and creating a Pipeline job to deploy the latest website code from Sarah's Gitea repository.

The objective was to configure Jenkins to execute deployment commands on App Server 1, pull the latest code from the `web_app` repository, and make the website accessible through the load balancer's main URL.

### Challenge Requirement

Set up an automated deployment pipeline using Jenkins with the following requirements:

1. Add a Jenkins agent named `App Server 1`.
2. Assign the label `stapp01`.
3. Set the remote root directory to `/home/sarah/jenkins_agent`.
4. Create a standard Pipeline job named `xfusion-webapp-job`.
5. Create a single stage named `Deploy`.
6. Pull the latest code from Sarah's `web_app` Gitea repository.
7. Deploy the website under `/var/www/html` on App Server 1.
8. Make sure the website loads through the load balancer's main URL without a `/web_app` subdirectory.

---

## Objectives

The objectives of this challenge are:

- Understand Jenkins agents and distributed builds.
- Configure App Server 1 as a Jenkins agent.
- Understand Jenkins node labels.
- Configure the agent's remote root directory.
- Create a standard Jenkins Pipeline job.
- Understand Jenkins Pipeline syntax.
- Connect Jenkins deployment with a Gitea repository.
- Use Git commands inside a Jenkins pipeline.
- Pull the latest source code from the `master` branch.
- Deploy website files into Apache's document root.
- Verify the deployment through the load balancer.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 77 |
| CI/CD Tool | Jenkins |
| Source Code Management | Gitea |
| Repository Owner | Sarah |
| Repository Name | `web_app` |
| Jenkins Agent | `App Server 1` |
| Agent Label | `stapp01` |
| Remote Root Directory | `/home/sarah/jenkins_agent` |
| Pipeline Job | `xfusion-webapp-job` |
| Job Type | Pipeline |
| Pipeline Stage | `Deploy` |
| Deployment Server | App Server 1 |
| Document Root | `/var/www/html` |
| Web Server | Apache |
| Apache Port | `8080` |
| Branch | `master` |
| Status | Completed |

---

# Solution

## Step 1: Access Jenkins

First, I opened Jenkins using the Jenkins button available in the KodeKloud lab environment.

I logged in using the provided administrator credentials.

After logging in, I accessed the Jenkins dashboard.

---

## Step 2: Configure App Server 1 as a Jenkins Agent

Next, I configured App Server 1 as a Jenkins agent so that Jenkins could execute deployment commands remotely.

I followed these steps:

1. Open Jenkins Dashboard.
2. Navigate to **Manage Jenkins**.
3. Open **Nodes**.
4. Click **New Node**.
5. Enter the node name:

```text
App Server 1
```

6. Select **Permanent Agent**.
7. Configure the following details:

| Configuration | Value |
|---|---|
| Node Name | `App Server 1` |
| Remote Root Directory | `/home/sarah/jenkins_agent` |
| Labels | `stapp01` |
| Usage | Use this node as much as possible |
| Launch Method | Launch agents via SSH |

8. Configure the SSH host as `stapp01`.
9. Select the provided SSH credentials for the `sarah` user.
10. Configure host key verification according to the lab environment.
11. Save the configuration.

The remote root directory is where Jenkins stores the agent's workspace and related files.

---

## Step 3: Verify the Jenkins Agent

After configuring the node, I checked its status.

The agent needed to be online and connected successfully.

I verified:

- Node name was `App Server 1`.
- Label was `stapp01`.
- Remote root directory was `/home/sarah/jenkins_agent`.
- Agent connection status was online.

This confirmed that Jenkins could execute builds on App Server 1.

---

## Step 4: Understand the Existing Repository

The `web_app` repository was already cloned on App Server 1 under:

```text
/var/www/html
```

This directory is the Apache document root.

The repository contains the website source code, and the Jenkins pipeline needs to pull the latest changes from the remote Gitea repository.

The important point is that the repository is already available in the document root.

Therefore, there is no need to clone it into another directory.

---

## Step 5: Create a New Jenkins Pipeline Job

After configuring the agent, I created the Jenkins pipeline job.

Steps:

1. Return to the Jenkins Dashboard.
2. Click **New Item**.
3. Enter the job name:

```text
xfusion-webapp-job
```

4. Select **Pipeline**.
5. Do not select Multibranch Pipeline.
6. Click OK.

This created a standard Jenkins Pipeline job.

---

## Step 6: Configure the Pipeline Script

I scrolled down to the Pipeline section.

I selected:

```text
Definition: Pipeline script
```

Then I added the following pipeline script:

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

### Understanding the Pipeline

**1. Agent**

```groovy
agent {
    label 'stapp01'
}
```

This tells Jenkins to execute the pipeline on the agent with the label `stapp01`.

**2. Stage**

```groovy
stage('Deploy')
```

This creates the required deployment stage.

**3. Deployment Commands**

```bash
cd /var/www/html
git pull origin master
```

The first command moves into the existing website repository.

The second command retrieves the latest changes from the remote `master` branch.

Because the repository is already cloned inside `/var/www/html`, the updated website files become available directly in Apache's document root.

---

## Step 7: Save and Execute the Pipeline

After entering the pipeline script, I clicked **Save**.

Then I followed these steps:

1. Opened `xfusion-webapp-job`.
2. Clicked **Build Now**.
3. Opened **Console Output**.
4. Checked the deployment logs.

The pipeline executed on App Server 1.

The important commands in the console output were:

```text
cd /var/www/html
git pull origin master
```

The build should finish with:

```text
Finished: SUCCESS
```

---

## Step 8: Verify the Website Deployment

After the pipeline executed successfully, I verified the website using the load balancer's main URL.

I clicked the **App** button in the KodeKloud lab environment.

The website needed to load directly from the main URL.

Correct:

```text
https://<load-balancer-url>/
```

Incorrect:

```text
https://<load-balancer-url>/web_app
```

This confirmed that the website was available directly from the Apache document root without an additional repository subdirectory.

---

# Jenkins Pipeline Workflow

The complete deployment process works as follows:

```text
        Jenkins Dashboard
                |
                |
       Configure Agent Node
                |
                |
          App Server 1
          Label: stapp01
                |
                |
       Create Pipeline Job
       xfusion-webapp-job
                |
                |
         Deploy Stage
                |
                |
        Execute Git Pull
                |
                |
       Gitea Repository
          sarah/web_app
                |
                |
       /var/www/html
                |
                |
        Apache Web Server
           Port 8080
                |
                |
          Load Balancer
                |
                |
        Website Accessible
```

---

# Troubleshooting and Important Points

During this task, the following points were important for successful deployment:

### 1. Agent Must Be Online

If the agent is offline, Jenkins cannot execute the pipeline on App Server 1.

Check the node configuration, SSH credentials, Java installation, and connectivity.

### 2. Correct Agent Label

The pipeline must use:

```groovy
agent {
    label 'stapp01'
}
```

If the label does not match the configured node, Jenkins may not find an available agent.

### 3. Existing Repository

The repository is already cloned under:

```text
/var/www/html
```

Therefore, use:

```bash
cd /var/www/html
git pull origin master
```

Avoid creating an unnecessary nested repository directory.

### 4. Avoid Unnecessary Sudo

The Jenkins agent runs using the configured `sarah` account.

The existing repository and document root are accessible to this user in the lab environment.

Therefore, unnecessary `sudo` commands can create permission or authentication problems.

### 5. Correct Pipeline Stage Name

The stage name must be exactly:

```text
Deploy
```

It is case-sensitive.

### 6. Verify the Main URL

Always check the website through the load balancer's main URL rather than accessing a nested `/web_app` directory.

---

# What I Learned

From this challenge, I learned and practiced:

- How Jenkins agents work.
- How to configure a remote Jenkins agent using SSH.
- How to assign labels to Jenkins nodes.
- How remote root directories work.
- How to create a standard Jenkins Pipeline job.
- How to write a declarative Jenkins pipeline.
- How to execute shell commands inside a pipeline.
- How Jenkins integrates with Gitea.
- How to use `git pull` for retrieving the latest code.
- How to deploy website code into Apache's document root.
- How Jenkins can automate deployment tasks.
- How to verify deployment using console output and a load balancer.
- Why correct directory structure and file permissions matter in CI/CD.

# Challenge Status

Day 77 — Completed Successfully ✅

**Jenkins Agent Configured. Pipeline Created. Website Deployment Automated.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
