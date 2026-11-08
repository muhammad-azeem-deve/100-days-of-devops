# Day 70 - Commands

This file contains the steps and configuration details used during **Day 70 of my 100 Days of DevOps Challenge**.

The objective was to create a Jenkins user named `mark`, assign Overall Read permission, and provide only Job Read permission on an existing Jenkins job.

---

# Step 1: Access Jenkins

Open Jenkins from the KodeKloud lab environment.

Log in using the administrator credentials provided by the lab.

---

# Step 2: Create a New User

Navigate to:

```text
Manage Jenkins → Users → Create User
```

Enter the following details:

```text
Username: mark
Password: Lab-provided password
Confirm Password: Same password
Full Name: Mark
```

Click Create User.

---

# Step 3: Install Matrix Authorization Strategy

Navigate to:

```text
Manage Jenkins → Plugins → Available Plugins
```

Search for:

```text
Matrix Authorization Strategy
```

Select the plugin and click Install.

If required, restart Jenkins using:

```text
Restart Jenkins when installation is complete
and no jobs are running
```

Wait until Jenkins becomes available again.

---

# Step 4: Configure Global Security

Navigate to:

```text
Manage Jenkins → Security
```

Under Authorization, select:

```text
Project-based Matrix Authorization Strategy
```

Configure the global permission matrix.

### Administrator

```text
User: admin
Permission: Overall → Administer
Status: Enabled
```

### New User

```text
User: mark
Permission: Overall → Read
Status: Enabled
```

### Anonymous User

```text
User: Anonymous
Permissions: All Disabled
```

Click Save.

---

# Step 5: Configure Existing Job Security

Return to the Jenkins Dashboard.

Open the existing Jenkins job.

Navigate to:

```text
Existing Job → Configure
```

Find and enable:

```text
Enable project-based security
```

---

# Step 6: Assign Job Read Permission

Inside the project-based security permission matrix, add:

```text
mark
```

Select only:

```text
Job → Read
```

Keep all other permissions unchecked.

Required configuration:

```text
User: mark

Job Permissions:

Read       ✓
Build      ✗
Configure  ✗
Delete     ✗
Workspace  ✗
Cancel     ✗
```

Click Save.

---

# Step 7: Verify Permissions

Log out from the administrator account.

Log in using:

```text
Username: mark
Password: Lab-provided password
```

Verify that:

```text
Jenkins Dashboard: Accessible
Existing Job: Readable
Job Configuration: Not Allowed
Build Execution: Not Allowed
Administrator Access: Not Allowed
```

---

# Permission Configuration Summary

## Global Permission Matrix

| User | Overall Read | Overall Administer |
|------|--------------|--------------------|
| admin | As configured | ✓ |
| mark | ✓ | ✗ |
| Anonymous | ✗ | ✗ |

## Job-Level Permission Matrix

| User | Job Read | Job Build | Job Configure | Job Delete |
|------|----------|-----------|---------------|------------|
| mark | ✓ | ✗ | ✗ | ✗ |

---

# Jenkins Navigation Summary

```text
1. Open Jenkins
   |
   v
2. Manage Jenkins → Users
   |
   v
3. Create User → mark
   |
   v
4. Manage Jenkins → Plugins
   |
   v
5. Install Matrix Authorization Strategy
   |
   v
6. Manage Jenkins → Security
   |
   v
7. Select Project-based Matrix Authorization Strategy
   |
   v
8. Give admin Overall Administer
   |
   v
9. Give mark Overall Read
   |
   v
10. Remove Anonymous permissions
   |
   v
11. Open Existing Jenkins Job → Configure
   |
   v
12. Enable Project-based Security
   |
   v
13. Add mark → Job Read only
   |
   v
14. Save and verify permissions
```

---

# Important Concepts

### Overall Read

Allows a user to access Jenkins and view the dashboard.

### Job Read

Allows a user to view a particular Jenkins job.

### Overall Administer

Provides administrator-level access to Jenkins.

### Project-based Matrix Authorization

Allows administrators to assign permissions globally and separately to individual Jenkins jobs.

### Principle of Least Privilege

Users should receive only the permissions necessary to perform their assigned tasks.

---

# Important Lesson

The most important part of this challenge was understanding the difference between global permissions and job-level permissions.

For this task:

```text
Global Permission:
mark → Overall Read
```

```text
Job-Level Permission:
mark → Job Read
```

The administrator retained:

```text
admin → Overall Administer
```

And Anonymous users received no permissions.

**Day 70 completed successfully.** 🚀
