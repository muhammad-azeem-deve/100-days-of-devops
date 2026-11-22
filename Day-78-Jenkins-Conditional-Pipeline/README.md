# Day 78 - Jenkins Conditional Pipeline

## Challenge Overview

This is Day 78 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Jenkins Conditional Pipelines and Automated Deployment**. The task required setting up an automated deployment pipeline for xFusionCorp's static website on **Nautilus App Server (App Server 1)** using Jenkins.

The objective was to configure App Server 1 as a Jenkins Agent, create a Pipeline job with a string parameter named `BRANCH`, and use conditional statements to deploy different versions of the website depending on the selected Git branch.

### Challenge Requirement

Set up a Jenkins Pipeline job named:

```text
nautilus-webapp-job
```

The pipeline must:

- Execute on App Server 1 using the label `stapp01`.
- Accept a string parameter named `BRANCH`.
- Deploy the `master` branch when `BRANCH=master`.
- Deploy the `feature` branch when `BRANCH=feature`.
- Deploy the website into `/var/www/html`.
- Run the deployment through a single stage named `Deploy`.
- Display the correct website content after deployment.

---

## Objectives

The objectives of this challenge are:

- Prepare App Server 1 for Jenkins Agent configuration.
- Install Java on App Server 1.
- Create the Jenkins agent working directory.
- Configure appropriate permissions on `/var/www/html`.
- Install the required Jenkins plugins.
- Configure App Server 1 as a Jenkins Agent.
- Assign the agent the label `stapp01`.
- Create a Pipeline job instead of a Multibranch Pipeline.
- Configure a string parameter named `BRANCH`.
- Write conditional pipeline logic using `if/else`.
- Deploy the `master` and `feature` branches.
- Verify deployment using the KodeKloud App button.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 78 |
| CI/CD Tool | Jenkins |
| Job Name | `nautilus-webapp-job` |
| Job Type | Pipeline |
| Agent Name | `App Server 1` |
| Agent Label | `stapp01` |
| Agent User | `sarah` |
| Agent Directory | `/home/sarah/jenkins_agent` |
| Deployment Directory | `/var/www/html` |
| Parameter | `BRANCH` |
| Branches | `master`, `feature` |
| Pipeline Stage | `Deploy` |
| Status | Completed |

---

# Solution

## Step 1: Access App Server 1

First, I accessed App Server 1 using SSH.

```bash
ssh sarah@stapp01
```

I verified the server:

```bash
hostname
```

This confirmed that I was working on the correct application server.

---

## Step 2: Install Java

Jenkins Agents require Java to run.

I installed Java on App Server 1:

```bash
sudo yum install java-11-openjdk -y
```

I verified the installation:

```bash
java -version
```

This confirmed that Java was available on the application server.

---

## Step 3: Create Jenkins Agent Directory

Next, I created the working directory for the Jenkins Agent.

```bash
sudo mkdir -p /home/sarah/jenkins_agent
```

I assigned ownership to Sarah:

```bash
sudo chown -R sarah:sarah /home/sarah/jenkins_agent
```

This directory is used as the remote working directory for the Jenkins Agent.

---

## Step 4: Configure Website Directory Permissions

The deployment process required Jenkins to write website files into:

```text
/var/www/html
```

I changed the ownership of the directory to the `sarah` user:

```bash
sudo chown -R sarah:sarah /var/www/html
```

This allowed the Jenkins Agent running as Sarah to update the website files.

---

## Step 5: Install Jenkins Plugins

After preparing App Server 1, I accessed the Jenkins dashboard.

I opened:

```text
Manage Jenkins → Plugins
```

I installed the required plugins for the task, including:

- Pipeline
- Git
- SSH Build Agents

These plugins provide pipeline functionality, Git integration, and SSH-based agent management.

---

## Step 6: Configure App Server 1 as a Jenkins Agent

Next, I configured App Server 1 as a Jenkins Agent.

I followed these steps:

1. Opened Manage Jenkins.
2. Navigated to Nodes.
3. Clicked New Node.
4. Entered the node name:

```text
App Server 1
```

5. Selected Permanent Agent.
6. Configured the remote root directory:

```text
/home/sarah/jenkins_agent
```

7. Assigned the agent label:

```text
stapp01
```

8. Configured the SSH connection using the `sarah` user and the appropriate credentials.
9. Saved the configuration.
10. Verified that the agent was connected and online.

The Jenkins Controller could now execute pipeline tasks on App Server 1.

---

## Step 7: Create a Jenkins Pipeline Job

I returned to the Jenkins dashboard and created a new job.

I followed these steps:

1. Clicked New Item.
2. Entered the job name:

```text
nautilus-webapp-job
```

3. Selected Pipeline.
4. Clicked OK.

**Important:** I selected a normal Pipeline job, not a Multibranch Pipeline.

---

## Step 8: Configure the String Parameter

Inside the job configuration, I enabled parameterization and added a String Parameter.

The parameter configuration was:

| Field | Value |
|---|---|
| Name | `BRANCH` |
| Default Value | `master` |
| Description | Git branch to deploy |

This parameter allows me to select which version of the website should be deployed.

---

## Step 9: Configure the Conditional Pipeline

I configured a single pipeline stage named:

```text
Deploy
```

The pipeline was configured to run on the Jenkins Agent with the label:

```text
stapp01
```

The conditional logic was:

- If the parameter is `master`, deploy the master branch.
- If the parameter is `feature`, deploy the feature branch.
- If another value is provided, stop the build with an error.

### Pipeline Script

The following is an illustrative Pipeline implementation. Replace `REPOSITORY_URL` with the actual `web_app` repository URL used in the challenge.

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

### Explanation

**Agent:**

```groovy
agent {
    label 'stapp01'
}
```

Runs the pipeline on App Server 1.

**Parameter:**

```groovy
string(name: 'BRANCH', defaultValue: 'master')
```

Allows the user to select the branch.

**Conditional statement:**

```groovy
if (params.BRANCH == 'master')
```

Checks whether the selected branch is master.

**Feature condition:**

```groovy
else if (params.BRANCH == 'feature')
```

Checks whether the selected branch is feature.

**Deployment directory:**

```text
/var/www/html
```

This is the directory where the website files are deployed.

The example uses `rsync` to synchronize the checked-out repository contents with the website directory. The deployment directory must be writable by the Jenkins Agent user, and `rsync` must be installed.

---

## Step 10: Test the Master Branch

After saving the pipeline configuration, I clicked:

```text
Build with Parameters
```

I selected:

```text
BRANCH = master
```

Then I started the build.

After successful execution, I clicked the KodeKloud App button.

The website displayed:

```text
Welcome to xFusionCorp Industries!
```

This confirmed that the master branch was deployed successfully.

---

## Step 11: Test the Feature Branch

Next, I ran the same Pipeline job again.

This time, I selected:

```text
BRANCH = feature
```

The pipeline checked out the feature branch and deployed its content.

After successful execution, I opened the website using the App button.

The website displayed:

```text
Updated
```

This confirmed that the feature branch was deployed successfully.

---

## Step 12: Restore the Master Branch

Finally, I ran the pipeline again with:

```text
BRANCH = master
```

The pipeline deployed the original website version again.

After checking the App button, the website displayed:

```text
Welcome to xFusionCorp Industries!
```

This confirmed that both branches could be deployed successfully using the same Jenkins Pipeline job.

---

# Deployment Verification

The following results confirmed that the conditional pipeline was working correctly.

| Build | BRANCH Parameter | Website Content |
|---|---|---|
| First Build | `master` | Welcome to xFusionCorp Industries! |
| Second Build | `feature` | Updated |
| Final Build | `master` | Welcome to xFusionCorp Industries! |

The website was accessible through the KodeKloud App button at the root URL without requiring a subdirectory.

---

# What I Learned

From this challenge, I learned and practiced:

- How Jenkins Controllers and Agents work together.
- How to prepare a Linux server as a Jenkins Agent.
- Why Java is required for Jenkins Agents.
- How to create a Jenkins Agent working directory.
- How Linux ownership and permissions affect deployment.
- How to install Jenkins plugins.
- How to configure an SSH-based Jenkins Agent.
- How to assign labels to Jenkins Agents.
- The difference between Pipeline and Multibranch Pipeline jobs.
- How to configure Jenkins build parameters.
- How to use string parameters in Jenkins.
- How to write conditional statements using `if/else` in Groovy.
- How to deploy different Git branches using Jenkins.
- How to automate static website deployment.
- How to verify deployments through the application URL.

# Challenge Status

Day 78 — Completed Successfully ✅

**One Jenkins Pipeline, two Git branches, and automated deployment!**

Successfully deployed both `master` and `feature` branches using a conditional Jenkins Pipeline on App Server 1.

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
