# Day 76 - Commands

This file contains the step-by-step navigation and configuration actions used during **Day 76 of my 100 Days of DevOps Challenge**.

The objective was to configure job-level permissions for `sam` and `rohan` on the Jenkins job named `Packages`.

**Note:** This challenge was completed through the Jenkins web interface. No Linux terminal commands or Jenkins CLI commands were required.

---

# Step 1: Login to Jenkins

Open the Jenkins web interface.

Login using:

```text
Username: admin
Password: Provided by KodeKloud
```

---

# Step 2: Install Matrix Authorization Strategy Plugin

Navigate to:

```text
Manage Jenkins
    |
    v
Plugins
    |
    v
Available Plugins
```

Search for:

```text
Matrix Authorization Strategy
```

Then:

1. Select the plugin.
2. Click Install.
3. Wait for installation to complete.

---

# Step 3: Open Global Security Settings

Navigate to:

```text
Manage Jenkins
    |
    v
Security
    |
    v
Configure Global Security
```

Find the Authorization section.

Select the project-based matrix authorization strategy if required by the current configuration.

Avoid changing unrelated global permissions.

---

# Step 4: Open Packages Job

Return to the Jenkins dashboard.

Find the job:

```text
Packages
```

Open it and click:

```text
Configure
```

---

# Step 5: Enable Project-Based Security

Inside the Packages job configuration:

Find and enable:

```text
Enable project-based security
```

---

# Step 6: Configure Inheritance Strategy

Find the inheritance strategy dropdown.

Select:

```text
Inherit permissions from parent ACL
```

---

# Step 7: Add Sam

In the project-based permission matrix, add:

```text
sam
```

Enable the following permissions:

```text
Job / Build       ✓
Job / Configure   ✓
Job / Read        ✓
```

Keep the other requested permissions disabled for Sam.

---

# Step 8: Add Rohan

Add the second developer:

```text
rohan
```

Enable:

```text
Job / Build       ✓
Job / Cancel      ✓
Job / Configure   ✓
Job / Read        ✓
Run / Update      ✓
SCM / Tag         ✓
```

The exact permission category labels may vary slightly depending on the Jenkins version.

---

# Step 9: Save Configuration

After configuring both developers:

1. Review the permission matrix.
2. Confirm the inheritance strategy.
3. Confirm the correct permissions for Sam.
4. Confirm the correct permissions for Rohan.
5. Click Save.

---

# Step 10: Verify Permissions

Reopen:

```text
Packages
    |
    v
Configure
    |
    v
Project-Based Security
```

Verify the following matrix:

| Permission | Sam | Rohan |
|------------|:---:|:---:|
| Build | ✓ | ✓ |
| Cancel | ✗ | ✓ |
| Configure | ✓ | ✓ |
| Read | ✓ | ✓ |
| Update | ✗ | ✓ |
| Tag | ✗ | ✓ |

Also verify:

```text
Project-Based Security: Enabled

Inheritance Strategy:
Inherit permissions from parent ACL
```

---

# Quick Navigation Summary

```text
1. Login to Jenkins as admin.

2. Manage Jenkins
   -> Plugins
   -> Available Plugins
   -> Matrix Authorization Strategy
   -> Install

3. Manage Jenkins
   -> Security
   -> Configure Global Security
   -> Authorization
   -> Project-based Matrix Authorization Strategy

4. Dashboard
   -> Packages
   -> Configure

5. Enable:
   Enable project-based security

6. Set inheritance:
   Inherit permissions from parent ACL

7. Add sam:
   Build
   Configure
   Read

8. Add rohan:
   Build
   Cancel
   Configure
   Read
   Update
   Tag

9. Click Save.

10. Reopen Packages configuration and verify.
```

---

# Important Lessons

- Global security controls authorization across Jenkins.
- Project-based security allows individual jobs to have their own permission matrix.
- The inheritance strategy determines how parent permissions are inherited.
- Different developers can receive different permissions on the same job.
- Always verify the permission matrix before saving.
- Avoid modifying unrelated jobs when a task requires access changes for only one job.

**Main Plugin:** Matrix Authorization Strategy

**Main Jenkins Job:** Packages

**Developers:** Sam and Rohan

**Challenge Status:** Completed Successfully ✅

100 Days. 100 Challenges. One DevOps Journey. 🚀
