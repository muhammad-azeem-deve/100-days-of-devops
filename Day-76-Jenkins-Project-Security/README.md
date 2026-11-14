# Day 76 - Jenkins Project Security

## Challenge Overview

This is Day 76 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Jenkins Project-Based Security and Matrix Authorization Strategy**. The task required configuring job-level permissions for two newly recruited developers, `sam` and `rohan`, on an existing Jenkins job named `Packages`.

The objective was to install the Matrix Authorization Strategy plugin, enable project-based security, configure the inheritance strategy, and assign specific permissions to each developer without modifying any other Jenkins jobs.

### Challenge Requirement

Configure job-level security for the existing Jenkins job named `Packages`.

Two developers required different permissions:

**Sam:**
- Build
- Configure
- Read

**Rohan:**
- Build
- Cancel
- Configure
- Read
- Update
- Tag

The inheritance strategy needed to be configured as:

```text
Inherit permissions from parent ACL
```

All permissions had to be configured specifically for the `Packages` job without affecting other jobs.

---

## Objectives

The objectives of this challenge are:

- Log in to Jenkins using the administrator account.
- Install the Matrix Authorization Strategy plugin.
- Enable project-based authorization.
- Locate the existing `Packages` job.
- Enable project-based security for the job.
- Configure the inheritance strategy.
- Grant Build, Configure, and Read permissions to Sam.
- Grant Build, Cancel, Configure, Read, Update, and Tag permissions to Rohan.
- Save and verify the permission configuration.
- Understand the difference between global security and project-based security.
- Configure permissions without affecting other Jenkins jobs.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 76 |
| Tool | Jenkins |
| Login User | `admin` |
| Plugin | Matrix Authorization Strategy |
| Existing Job | `Packages` |
| Developer 1 | `sam` |
| Developer 2 | `rohan` |
| Security Type | Project-Based Security |
| Inheritance Strategy | Inherit permissions from parent ACL |
| Status | Completed |

---

# Solution

## Step 1: Login to Jenkins

First, I opened the Jenkins web interface and logged in using the administrator account.

Credentials:

```text
Username: admin
Password: Provided by KodeKloud
```

After successful login, I accessed the Jenkins dashboard.

---

## Step 2: Install Matrix Authorization Strategy Plugin

Next, I installed the required authorization plugin.

Steps:

1. Open **Manage Jenkins**.
2. Click **Plugins**.
3. Navigate to Available Plugins.
4. Search for:

```text
Matrix Authorization Strategy
```

5. Select the plugin.
6. Click Install.

The plugin provides matrix-based authorization and allows permissions to be assigned to individual users.

---

## Step 3: Configure Global Authorization Strategy

After installing the plugin, I accessed the Jenkins security configuration.

Navigation:

```text
Manage Jenkins
       |
       v
Security
       |
       v
Configure Global Security
```

Under the Authorization section, I selected the project-based matrix authorization strategy, if required by the existing Jenkins configuration.

This enabled project-level permission management.

I avoided changing unrelated global permissions.

---

## Step 4: Open the Packages Job

After configuring the authorization strategy, I returned to the Jenkins dashboard.

I located the existing job:

```text
Packages
```

Then I opened the job and clicked:

```text
Configure
```

This opened the configuration settings for the Packages job.

---

## Step 5: Enable Project-Based Security

Inside the Packages job configuration, I located the project-based security option.

I enabled:

```text
Enable project-based security
```

After enabling it, I selected the required inheritance strategy:

```text
Inherit permissions from parent ACL
```

This allows the job to inherit permissions from its parent authorization configuration while supporting additional job-level permissions.

---

## Step 6: Configure Permissions for Sam

Next, I added the developer:

```text
sam
```

I granted the following permissions:

| Permission | Access |
|------------|--------|
| Build | Enabled |
| Configure | Enabled |
| Read | Enabled |
| Cancel | Disabled |
| Update | Disabled |
| Tag | Disabled |

Sam was given only the three required permissions.

---

## Step 7: Configure Permissions for Rohan

After configuring Sam, I added the second developer:

```text
rohan
```

I granted the following permissions:

| Permission | Access |
|------------|--------|
| Build | Enabled |
| Cancel | Enabled |
| Configure | Enabled |
| Read | Enabled |
| Update | Enabled |
| Tag | Enabled |

Rohan received all six required permissions.

---

## Step 8: Save the Configuration

After configuring both developers, I reviewed the permission matrix.

I confirmed:

- Sam had Build, Configure, and Read permissions.
- Rohan had Build, Cancel, Configure, Read, Update, and Tag permissions.
- The inheritance strategy was set to Inherit permissions from parent ACL.
- Project-based security was enabled.
- The configuration was applied to the existing Packages job only.

Finally, I clicked:

```text
Save
```

---

## Step 9: Verify the Configuration

After saving, I reopened the Packages job configuration.

I verified that the permissions were correctly assigned to both developers.

### Final Permission Matrix

| Permission | Sam | Rohan |
|------------|:---:|:---:|
| Build | ✓ | ✓ |
| Cancel | ✗ | ✓ |
| Configure | ✓ | ✓ |
| Read | ✓ | ✓ |
| Update | ✗ | ✓ |
| Tag | ✗ | ✓ |

The configuration was limited to the Packages job, and no other jobs were intentionally modified.

---

# Understanding Jenkins Project-Based Security

Jenkins project-based security allows administrators to assign different permissions to users for individual jobs.

For example:

```text
Jenkins
   |
   |--- Packages Job
   |       |
   |       |--- Sam
   |       |     |--- Build
   |       |     |--- Configure
   |       |     |--- Read
   |       |
   |       |--- Rohan
   |             |--- Build
   |             |--- Cancel
   |             |--- Configure
   |             |--- Read
   |             |--- Update
   |             |--- Tag
   |
   |--- Other Jobs
           |
           |--- Existing permissions remain unchanged
```

This approach is useful when different developers need different access levels for different projects.

---

# Understanding Inheritance Strategy

The selected inheritance strategy was:

```text
Inherit permissions from parent ACL
```

### What does it mean?

It means the Packages job inherits applicable permissions from its parent Jenkins authorization configuration.

The job can also have additional permissions assigned specifically to its own permission matrix.

This helps maintain existing access rules while providing additional job-level access.

---

# What I Learned

From this challenge, I learned and practiced:

- How Jenkins user authorization works.
- How to install the Matrix Authorization Strategy plugin.
- How to configure Jenkins global authorization.
- How to enable project-based security for an individual job.
- How to configure the inheritance strategy.
- How to assign different permissions to different developers.
- The meaning of Build, Configure, Read, Cancel, Update, and Tag permissions.
- How to configure job-level access without intentionally changing other jobs.
- The difference between global authorization and project-based authorization.
- Why access control is important in a collaborative DevOps environment.

# Challenge Status

Day 76 — Completed Successfully ✅

**Jenkins Project-Based Security configured for Sam and Rohan on the Packages job.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
