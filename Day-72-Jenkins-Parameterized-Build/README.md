# Day 72 - Jenkins Parameterized Builds

## Challenge Overview

This is Day 72 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Jenkins Parameterized Builds**. The objective was to create a parameterized Jenkins job that accepts user input through String and Choice parameters and displays the provided values during build execution.

This challenge helped me understand how Jenkins parameters make jobs flexible and reusable by allowing users to provide different values whenever they execute a build.

### Challenge Requirement

Create a parameterized Jenkins job named `parameterized-job` with the following requirements:

1. Create a Freestyle project named `parameterized-job`.
2. Add a String Parameter named `Stage` with default value `Build`.
3. Add a Choice Parameter named `env` with the following options:
   - Development
   - Staging
   - Production
4. Configure the job to execute a shell command that displays both parameter values.
5. Run the Jenkins job at least once with the choice parameter set to `Production`.
6. Verify the build through Console Output.

---

## Objectives

The objectives of this challenge are:

- Access Jenkins through its web interface.
- Create a new Freestyle project.
- Enable parameterized builds.
- Add a String Parameter.
- Configure a default parameter value.
- Add a Choice Parameter with multiple options.
- Execute shell commands using Jenkins parameters.
- Run a build with custom parameter values.
- Verify the build using Console Output.
- Understand how parameterized builds make Jenkins jobs reusable.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 72 |
| Tool | Jenkins |
| Job Name | `parameterized-job` |
| Job Type | Freestyle Project |
| String Parameter | `Stage` |
| Default Value | `Build` |
| Choice Parameter | `env` |
| Choices | Development, Staging, Production |
| Build Step | Execute Shell |
| Required Environment | Production |
| Status | Completed |

---

# Solution

## Step 1: Access Jenkins UI

First, I accessed the Jenkins UI by clicking the Jenkins button on the top bar of the KodeKloud lab.

I logged in using the provided credentials:

```text
Username: admin
Password: Adm!n321
```

After successful login, I reached the Jenkins dashboard.

---

## Step 2: Create a New Jenkins Job

On the Jenkins dashboard, I clicked **New Item**.

I entered the following job name:

```text
parameterized-job
```

Then I selected:

```text
Freestyle project
```

Finally, I clicked **OK** to create the job.

---

## Step 3: Enable Parameterized Builds

On the job configuration page, I located the **General** section.

I enabled the checkbox:

```text
This project is parameterized
```

This option allows the Jenkins job to accept input parameters before executing a build.

---

## Step 4: Add a String Parameter

I clicked:

```text
Add Parameter → String Parameter
```

Then I configured the parameter as follows:

| Field | Value |
|-------|-------|
| Name | `Stage` |
| Default Value | `Build` |
| Description | Stage of the build process |

The String Parameter allows users to provide a custom text value before running the job.

For this challenge, the default value was `Build`.

---

## Step 5: Add a Choice Parameter

Next, I clicked:

```text
Add Parameter → Choice Parameter
```

I configured the parameter using these values:

| Field | Value |
|-------|-------|
| Name | `env` |
| Choices | Development, Staging, Production |
| Description | Target environment |

The choices were entered on separate lines:

```text
Development
Staging
Production
```

This created a dropdown menu from which users could select the target environment.

---

## Step 6: Configure the Build Step

After configuring both parameters, I scrolled down to the **Build** section.

I selected:

```text
Add build step → Execute shell
```

Then I added the following shell script:

```bash
echo "Stage: $Stage"
echo "Environment: $env"
```

### Understanding the Script

- `echo` displays text in the console.
- `$Stage` retrieves the value of the String Parameter.
- `$env` retrieves the selected Choice Parameter.

For example, if the selected values are:

```text
Stage = Build
env = Production
```

The script displays:

```text
Stage: Build
Environment: Production
```

---

## Step 7: Save the Jenkins Job

After configuring the parameters and build step, I clicked **Save**.

The job was successfully created with the name:

```text
parameterized-job
```

---

## Step 8: Build with Parameters

I opened the newly created job.

Instead of using the normal Build Now option, I clicked:

```text
Build with Parameters
```

Jenkins displayed the parameter input form.

I selected the following values:

| Parameter | Selected Value |
|-----------|----------------|
| Stage | Build |
| env | Production |

After selecting the values, I clicked **Build**.

This started the Jenkins job using the provided parameters.

---

## Step 9: Verify the Build

After executing the job, I opened the latest build number from the Build History section.

Then I clicked:

```text
Console Output
```

I verified that the output displayed both parameter values:

```text
Stage: Build
Environment: Production
```

The build completed successfully:

```text
Finished: SUCCESS
```

This confirmed that Jenkins was correctly receiving and using the configured parameters.

---

# Understanding Jenkins Parameters

Jenkins parameters allow users to provide input values before a job starts.

### 1. String Parameter

A String Parameter accepts text entered by the user.

Example:

```text
Name: Stage
Default Value: Build
```

It can be used for:

- Build stages
- Application versions
- Branch names
- Custom messages

### 2. Choice Parameter

A Choice Parameter provides predefined options in a dropdown menu.

Example:

```text
Name: env

Development
Staging
Production
```

It can be used for:

- Selecting deployment environments
- Choosing application configurations
- Selecting build types

---

# What I Learned

From this challenge, I learned and practiced:

- How to access and use the Jenkins dashboard.
- How to create a Freestyle project.
- What parameterized builds are.
- How to enable the `This project is parameterized` option.
- How to configure String Parameters.
- How to configure Choice Parameters.
- How to assign default values to parameters.
- How to use Jenkins parameters inside shell scripts.
- How to access parameter values using environment variables.
- How to execute a build using Build with Parameters.
- How to verify build results using Console Output.
- How parameterized builds make CI/CD jobs flexible and reusable.

# Challenge Status

Day 72 — Completed Successfully ✅

**Created a parameterized Jenkins job, configured String and Choice parameters, and successfully executed a build using the Production environment.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
