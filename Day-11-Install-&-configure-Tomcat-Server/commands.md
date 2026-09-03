# Day 11 - Tomcat Server Commands

This file contains the commands used to complete **Day 11** of the KodeKloud 100 Days of DevOps Challenge.

---

## 1. Connect to App Server 3

```bash
ssh <user>@stapp03
```

Check hostname:

```bash
hostname
```

Expected:

```text
stapp03
```

---

## 2. Install Tomcat

```bash
sudo dnf install -y tomcat
```

Verify installation:

```bash
rpm -qa | grep tomcat
```

---

## 3. Configure Tomcat Port

Open the Tomcat configuration file:

```bash
sudo vi /etc/tomcat/server.xml
```

Find the HTTP Connector:

```xml
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

Change:

```text
8080
```

to:

```text
5002
```

Final configuration:

```xml
<Connector port="5002" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

---

## 4. Enable Tomcat

```bash
sudo systemctl enable tomcat
```

---

## 5. Start Tomcat

```bash
sudo systemctl start tomcat
```

---

## 6. Check Tomcat Status

```bash
sudo systemctl status tomcat
```

Expected:

```text
active (running)
```

---

## 7. Verify Port 5002

```bash
sudo ss -tulnp | grep 5002
```

---

## 8. Copy ROOT.war from Jump Host

The WAR file was available at:

```text
/tmp/ROOT.war
```

Copy it to App Server 3:

```bash
scp /tmp/ROOT.war <user>@stapp03:/tmp/
```

---

## 9. Verify ROOT.war

On App Server 3:

```bash
ls -lh /tmp/ROOT.war
```

---

## 10. Deploy ROOT.war

Copy the WAR file to Tomcat's webapps directory:

```bash
sudo cp /tmp/ROOT.war /var/lib/tomcat/webapps/
```

Verify:

```bash
ls -lh /var/lib/tomcat/webapps/
```

---

## 11. Restart Tomcat

```bash
sudo systemctl restart tomcat
```

Check status:

```bash
sudo systemctl status tomcat
```

---

## 12. Verify ROOT Application

```bash
ls -la /var/lib/tomcat/webapps/
```

Expected files/directories include:

```text
ROOT.war
ROOT/
```

---

## 13. Test the Application

From the Jump Host:

```bash
curl http://stapp03:5002
```

You can also check the HTTP response:

```bash
curl -I http://stapp03:5002
```

---

## 14. Troubleshooting Commands

### Check Tomcat Service

```bash
sudo systemctl status tomcat
```

### View Tomcat Logs

```bash
sudo journalctl -u tomcat
```

### Check Port

```bash
sudo ss -tulnp | grep 5002
```

### Check Tomcat Configuration

```bash
sudo grep -n "Connector" /etc/tomcat/server.xml
```

### Check Deployed Application

```bash
ls -la /var/lib/tomcat/webapps/
```

---

# Quick Command Summary

```bash
# Connect to App Server 3
ssh <user>@stapp03

# Install Tomcat
sudo dnf install -y tomcat

# Verify installation
rpm -qa | grep tomcat

# Configure Tomcat
sudo vi /etc/tomcat/server.xml

# Enable Tomcat
sudo systemctl enable tomcat

# Start Tomcat
sudo systemctl start tomcat

# Check status
sudo systemctl status tomcat

# Check port 5002
sudo ss -tulnp | grep 5002

# Copy ROOT.war from Jump Host
scp /tmp/ROOT.war <user>@stapp03:/tmp/

# Deploy WAR
sudo cp /tmp/ROOT.war /var/lib/tomcat/webapps/

# Verify deployment
ls -la /var/lib/tomcat/webapps/

# Restart Tomcat
sudo systemctl restart tomcat

# Verify application
curl http://stapp03:5002
```

---

# Important Lesson

The key part of this challenge was understanding that the Tomcat port and application deployment are two separate configurations:

```text
Tomcat Configuration
        |
        +-- server.xml
        |
        +-- Port 5002
```

and:

```text
Application Deployment
        |
        +-- ROOT.war
        |
        +-- /var/lib/tomcat/webapps/
        |
        +-- ROOT application
```

Because the application was deployed as `ROOT.war`, it could be accessed directly from:

```text
http://stapp03:5002
```

without adding an application name to the URL.
