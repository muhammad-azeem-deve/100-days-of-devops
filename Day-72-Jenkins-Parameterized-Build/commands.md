# Day 72 - Commands

This file contains the configuration details and shell commands used during **Day 72 of my 100 Days of DevOps Challenge**.

The objective was to create a parameterized Jenkins Freestyle job named `parameterized-job` with String and Choice parameters.

---

# Step 1: Access Jenkins

Open the Jenkins UI from the KodeKloud lab.

Login credentials:

```text
Username: admin
Password: Adm!n321
```

---

# Step 2: Create a New Job

1. Click **New Item**.
2. Enter the job name:

```text
parameterized-job
```

3. Select:

```text
Freestyle project
```

4. Click **OK**.

---

# Step 3: Enable Parameterized Builds

In the General section, enable:

```text
This project is parameterized
```

---

# Step 4: Add String Parameter

Navigate to:

```text
Add Parameter → String Parameter
```

Configure:

```text
Name: Stage
Default Value: Build
Description: Stage of the build process
```

---

# Step 5: Add Choice Parameter

Navigate to:

```text
Add Parameter → Choice Parameter
```

Configure:

```text
Name: env
Description: Target environment
```

Enter the following choices on separate lines:

```text
Development
Staging
Production
```

---

# Step 6: Configure Execute Shell

Under the Build section, select:

```text
Add build step → Execute shell
```

Enter the following commands:

```bash
echo "Stage: $Stage"
echo "Environment: $env"
```

These commands display the parameter values supplied during build execution.

---

# Step 7: Save the Job

Click:

```text
Save
```

The Jenkins job will be available with the name:

```text
parameterized-job
```

---

# Step 8: Build with Parameters

Open the `parameterized-job`.

Click:

```text
Build with Parameters
```

Select:

```text
Stage: Build
env: Production
```

Click:

```text
Build
```

---

# Step 9: Verify Build Output

Open the latest build from Build History.

Select:

```text
Console Output
```

Expected output:

```text
Stage: Build
Environment: Production
Finished: SUCCESS
```

---

# Quick Configuration Summary

## Job Configuration

```text
Job Name: parameterized-job
Job Type: Freestyle project
Parameterized: Enabled
```

## String Parameter

```text
Name: Stage
Default Value: Build
```

## Choice Parameter

```text
Name: env

Choices:
Development
Staging
Production
```

## Execute Shell Script

```bash
echo "Stage: $Stage"
echo "Environment: $env"
```

## Build Parameters

```text
Stage: Build
env: Production
```

---

# Understanding Jenkins Parameters

| Parameter | Purpose |
|-----------|---------|
| String Parameter | Accepts user-provided text |
| Choice Parameter | Provides predefined dropdown options |
| `$Stage` | Accesses the Stage parameter |
| `$env` | Accesses the environment parameter |
| `echo` | Displays output in the console |

---

# Important Verification Checklist

- [ ] Jenkins dashboard accessed successfully.
- [ ] Job `parameterized-job` created.
- [ ] Freestyle project selected.
- [ ] Parameterized build option enabled.
- [ ] String parameter `Stage` created.
- [ ] Default value set to `Build`.
- [ ] Choice parameter `env` created.
- [ ] All three environment choices added.
- [ ] Execute Shell build step configured.
- [ ] Build executed with `Production`.
- [ ] Console Output verified.
- [ ] Build completed successfully.

---

# Important Lesson

A parameterized Jenkins job allows users to provide different input values every time they execute a build.

Instead of creating separate Jenkins jobs for Development, Staging, and Production, we can create one reusable job and select the required environment at runtime.

The most important part of this challenge was:

```bash
echo "Stage: $Stage"
echo "Environment: $env"
```

These commands demonstrate how Jenkins parameters can be accessed inside shell scripts.

**Day 72 completed successfully.** 🚀
