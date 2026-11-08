# Day 70 - Configure Jenkins User Access

## Challenge Overview

This is Day 70 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Jenkins User Management and Access Control**. The task required creating a new Jenkins user named `mark` and assigning limited read-only permissions using the **Project-based Matrix Authorization Strategy**.

The objective was to give `mark` overall read permission to access Jenkins and read permission only to an existing Jenkins job, without allowing the user to modify, build, or administer jobs.

### Challenge Requirement

Configure Jenkins access for a new user named `mark`.

The task required:

- Create a Jenkins user named `mark`.
- Install the Matrix Authorization Strategy plugin.
- Enable Project-based Matrix Authorization Strategy.
- Give `mark` Overall Read permission.
- Remove all permissions from Anonymous users.
- Ensure the administrator retains Overall Administer permission.
- Give `mark` only Read permission on the existing Jenkins job.
- Verify the user's permissions by logging in as `mark`.

---

## Objectives

The objectives of this challenge are:

- Understand Jenkins user management.
- Create a new Jenkins user.
- Install the Matrix Authorization Strategy plugin.
- Configure global Jenkins permissions.
- Enable Project-based Matrix Authorization Strategy.
- Give users specific permissions using a permission matrix.
- Configure job-level security.
- Give a user read-only access to a particular job.
- Remove unnecessary Anonymous permissions.
- Understand the Principle of Least Privilege.
- Verify user access by logging in with the newly created account.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 70 |
| Tool | Jenkins |
| New Username | `mark` |
| Full Name | `Mark` |
| Authorization Strategy | Project-based Matrix Authorization |
| Global Permission | Overall Read |
| Job Permission | Job Read |
| Administrator | Overall Administer |
| Anonymous | No Permissions |
| Status | Completed |

---

# Solution

## Step 1: Access Jenkins

First, I accessed the Jenkins dashboard through the Jenkins button in the KodeKloud lab environment.

I logged in using the provided administrator credentials.

The administrator account was required to create users and configure Jenkins security settings.

---

## Step 2: Create a New Jenkins User

After accessing Jenkins, I navigated to:

```text
Manage Jenkins → Users
```

Then I clicked on:

```text
Create User
```

I entered the following details:

| Field | Value |
|-------|-------|
| Username | `mark` |
| Password | Lab-provided password |
| Confirm Password | Same password |
| Full Name | `Mark` |

After entering the information, I clicked the **Create User** button.

This successfully created the new Jenkins user.

---

## Step 3: Install Matrix Authorization Strategy Plugin

Next, I installed the plugin required for managing individual user permissions.

I navigated to:

```text
Manage Jenkins → Plugins → Available Plugins
```

I searched for:

```text
Matrix Authorization Strategy
```

Then I selected the plugin and clicked **Install**.

If Jenkins required a restart, I selected:

```text
Restart Jenkins when installation is complete
and no jobs are running
```

After restarting, I waited for Jenkins to become available again.

---

## Step 4: Configure Global Security

After installing the plugin, I navigated to:

```text
Manage Jenkins → Security
```

Under the **Authorization** section, I selected:

```text
Project-based Matrix Authorization Strategy
```

This authorization strategy allows permissions to be configured globally as well as individually for Jenkins jobs.

### Configure the Global Permission Matrix

I configured the permissions as follows:

| User | Permission | Status |
|------|------------|--------|
| admin | Overall → Administer | Enabled |
| mark | Overall → Read | Enabled |
| Anonymous | All permissions | Disabled |

Important configurations:

- The `admin` user retained administrator permissions.
- The `mark` user received only Overall Read permission.
- All permissions for Anonymous users were removed.

I then clicked **Save** to apply the global security configuration.

---

## Step 5: Configure Existing Job Security

After configuring global permissions, I returned to the Jenkins Dashboard.

I opened the existing Jenkins job and clicked:

```text
Configure
```

Inside the job configuration, I located:

```text
Enable project-based security
```

I enabled this option.

This allowed me to configure permissions specifically for the selected job.

---

## Step 6: Give Read Permission to mark

Inside the job's project-based security permission matrix, I added the user:

```text
mark
```

Then I selected only the following permission:

```text
Job → Read
```

I made sure that all other permissions for `mark` remained unchecked.

The required permission configuration was:

| Job Permission | mark |
|----------------|------|
| Read | Enabled |
| Build | Disabled |
| Configure | Disabled |
| Delete | Disabled |
| Workspace | Disabled |
| Cancel | Disabled |

I then clicked **Save** to apply the job-level permissions.

---

## Step 7: Verify User Permissions

After configuring global and job-level permissions, I logged out of the administrator account.

Then I logged in using the newly created account:

```text
Username: mark
Password: Lab-provided password
```

I verified the following:

- The user could access the Jenkins dashboard.
- The user could view the specified existing job.
- The user could not configure the job.
- The user could not trigger a new build.
- The user did not have administrator permissions.

This confirmed that the user had the required limited read-only access.

---

# Understanding Jenkins Permissions

Jenkins provides different permission categories.

### 1. Overall Permissions

These permissions control access to Jenkins globally.

For this challenge, the required permission was:

```text
Overall → Read
```

It allows the user to access and view Jenkins.

### 2. Job Permissions

These permissions control access to individual Jenkins jobs.

For this challenge, the required permission was:

```text
Job → Read
```

It allows the user to view the specified job without modifying or executing it.

### 3. Administrator Permissions

The administrator requires:

```text
Overall → Administer
```

This provides full administrative control over Jenkins.

### 4. Anonymous Permissions

Anonymous represents users who are not logged in.

For this challenge, all Anonymous permissions were removed to prevent unauthenticated access.

---

# What I Learned

From this challenge, I learned and practiced:

- How to create users in Jenkins.
- How to install the Matrix Authorization Strategy plugin.
- How Jenkins authentication and authorization work.
- How to configure Project-based Matrix Authorization Strategy.
- How to assign Overall Read permission.
- How to configure job-level permissions.
- How to give a user read-only access to a particular job.
- How to remove permissions from Anonymous users.
- How to preserve administrator access.
- The difference between global and job-level permissions.
- The importance of the Principle of Least Privilege.
- How to verify Jenkins access using different user accounts.

# Challenge Status

Day 70 — Completed Successfully ✅

**Jenkins user `mark` configured with Overall Read permission and Job Read permission on the specified existing job.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
