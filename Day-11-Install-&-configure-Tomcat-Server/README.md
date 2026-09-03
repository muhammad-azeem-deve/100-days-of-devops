# Day 11 - Install and Configure Tomcat Server

## Challenge Overview

This is Day 11 of my **100 Days of DevOps Challenge by KodeKloud**.

The Nautilus application development team completed the beta version of a Java-based application. The application needs to be deployed on **App Server 3** using the **Tomcat application server**.

The task involved installing Tomcat, configuring it to use a custom port, deploying the provided `ROOT.war` file, and verifying that the application was accessible directly from the base URL.

---

## Challenge Requirements

The following requirements had to be completed:

### a. Install Tomcat

Install the **Tomcat application server** on **App Server 3**.

### b. Configure Tomcat Port

Configure Tomcat to listen on:

```text
Port: 5002
```

### c. Deploy ROOT.war

A `ROOT.war` file is available on the **Jump Host** at:

```text
/tmp/ROOT.war
```

Deploy this WAR file to Tomcat on App Server 3.

The application must be accessible directly from the base URL:

```text
curl http://stapp03:5002
```

---

## Environment

| Component          | Details                      |
| ------------------ | ---------------------------- |
| Challenge          | Day 11                       |
| Application Server | App Server 3                 |
| Server Hostname    | `stapp03`                    |
| Application Server | Apache Tomcat                |
| Tomcat Port        | `5002`                       |
| WAR File           | `ROOT.war`                   |
| WAR Location       | `/tmp/ROOT.war` on Jump Host |
| Application URL    | `http://stapp03:5002`        |

---

## Solution

### Step 1: Connect to App Server 3

First, connect to App Server 3 from the Jump Host.

```bash
ssh <user>@stapp03
```

Verify the server:

```bash
hostname
```

The hostname should show:

```text
stapp03
```

---

### Step 2: Install Tomcat

Install the Tomcat package on App Server 3.

```bash
sudo dnf install -y tomcat
```

Verify that Tomcat was installed:

```bash
rpm -qa | grep tomcat
```

---

### Step 3: Configure Tomcat to Use Port 5002

Tomcat's configuration file is located at:

```text
/etc/tomcat/server.xml
```

Open the configuration file:

```bash
sudo vi /etc/tomcat/server.xml
```

Find the HTTP Connector configuration:

```xml
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

Change the port from `8080` to `5002`:

```xml
<Connector port="5002" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

Save and exit the file.

---

### Step 4: Enable and Start Tomcat

Enable Tomcat so that it starts automatically after a reboot:

```bash
sudo systemctl enable tomcat
```

Start the Tomcat service:

```bash
sudo systemctl start tomcat
```

Check the service status:

```bash
sudo systemctl status tomcat
```

Tomcat should be in the:

```text
active (running)
```

state.

---

### Step 5: Verify Tomcat Is Listening on Port 5002

Check whether Tomcat is listening on port `5002`:

```bash
sudo ss -tulnp | grep 5002
```

You should see Tomcat listening on the configured port.

---

## Step 6: Copy ROOT.war from the Jump Host

The `ROOT.war` file is available on the Jump Host at:

```text
/tmp/ROOT.war
```

From the Jump Host, copy the WAR file to App Server 3:

```bash
scp /tmp/ROOT.war <user>@stapp03:/tmp/
```

Then connect to App Server 3:

```bash
ssh <user>@stapp03
```

Verify the file:

```bash
ls -lh /tmp/ROOT.war
```

---

## Step 7: Deploy ROOT.war to Tomcat

Copy the WAR file into Tomcat's webapps directory:

```bash
sudo cp /tmp/ROOT.war /var/lib/tomcat/webapps/
```

Verify:

```bash
ls -lh /var/lib/tomcat/webapps/
```

The directory should contain:

```text
ROOT.war
```

---

## Step 8: Restart Tomcat

Restart Tomcat so that it detects and deploys the new WAR file:

```bash
sudo systemctl restart tomcat
```

Check the status:

```bash
sudo systemctl status tomcat
```

Tomcat should again show:

```text
active (running)
```

---

## Step 9: Verify ROOT Application Deployment

Tomcat should automatically extract the WAR file into a `ROOT` directory.

Check:

```bash
ls -la /var/lib/tomcat/webapps/
```

You should see something similar to:

```text
ROOT
ROOT.war
```

The `ROOT` application is important because it allows the application to open directly at:

```text
http://stapp03:5002
```

instead of requiring an additional application path.

---

## Step 10: Test the Application

From the Jump Host, test the application using `curl`:

```bash
curl http://stapp03:5002
```

If the deployment is successful, the response should contain the webpage/application content from `ROOT.war`.

You can also test the port:

```bash
curl -I http://stapp03:5002
```

A successful HTTP response confirms that Tomcat is serving the application.

---

## Troubleshooting

### Tomcat Service Is Not Running

Check the service:

```bash
sudo systemctl status tomcat
```

Check the logs:

```bash
sudo journalctl -u tomcat
```

---

### Port 5002 Is Not Listening

Check:

```bash
sudo ss -tulnp | grep 5002
```

If nothing is returned, verify `/etc/tomcat/server.xml` and make sure the Connector is configured with:

```xml
port="5002"
```

Then restart Tomcat:

```bash
sudo systemctl restart tomcat
```

---

### ROOT.war Is Not Deployed

Check the Tomcat webapps directory:

```bash
ls -la /var/lib/tomcat/webapps/
```

Make sure the file exists:

```text
ROOT.war
```

If necessary, copy it again:

```bash
sudo cp /tmp/ROOT.war /var/lib/tomcat/webapps/
```

Then restart:

```bash
sudo systemctl restart tomcat
```

---

## Final Verification

The main verification command for this challenge was:

```bash
curl http://stapp03:5002
```

The expected result is the webpage/application content provided inside `ROOT.war`.

The final setup was:

```text
App Server 3
     |
     +-- Tomcat
     |
     +-- Port 5002
     |
     +-- ROOT.war
     |
     +-- /var/lib/tomcat/webapps/ROOT.war
     |
     +-- http://stapp03:5002
```

---

## What I Learned

Through this challenge, I learned:

* How to install **Apache Tomcat** on a Linux server.
* How to configure Tomcat's HTTP connector.
* How to change Tomcat from its default port to a custom port.
* How to manage Tomcat using `systemctl`.
* How to deploy a Java WAR application.
* How `ROOT.war` is used for deployment at the application's base URL.
* How to verify a web application using `curl`.
* How to troubleshoot Tomcat service and port-related issues.

---

## Challenge Status

```text
Day 11: Completed
Tomcat Installation: Done
Port Configuration: 5002
ROOT.war Deployment: Done
Application Verification: Successful
```

---

## Conclusion

Day 11 was focused on deploying a Java-based application using **Apache Tomcat**.

I installed Tomcat on App Server 3, changed its HTTP port to `5002`, deployed the provided `ROOT.war` file, restarted the service, and verified the application using:

```bash
curl http://stapp03:5002
```

This challenge gave me practical experience with Java application deployment, Tomcat configuration, Linux services, and application troubleshooting.
