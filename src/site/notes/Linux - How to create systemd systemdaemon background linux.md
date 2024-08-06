---
{"dg-publish":true,"permalink":"/linux-how-to-create-systemd-systemdaemon-background-linux/"}
---

Systemd is the init system used by many modern Linux distributions to manage system processes and services. Creating a systemd service allows you to run background tasks, manage system daemons, and ensure that your services start automatically at boot. In this guide, we’ll walk through the steps to create a systemd service, specifically using an example of a Metabase daemon. Follow these steps to set up your own systemd service:

### **Step 1: Locate User-Defined Services**

Systemd services are typically stored in specific directories based on their scope. For user-defined services, you’ll usually work within `/etc/systemd/system/` on Ubuntu. This directory is where you should place your custom service files.

```bash
cd /etc/systemd/system/
```

### **Step 2: Create Your Service File**

Using your preferred text editor, create a new service file. You can name this file anything you like, but it must end with `.service`. For example, let’s name it `metabase.service`.

```bash
sudo nano whatever_you_want.service
```

### **Step 3: Define Your Service Configuration**

Add the following template to your service file. This configuration is designed for a hypothetical Metabase daemon, but you can customize it for any service you need.

```ini
[Unit]
Description=Metabase Daemon

[Service]
PIDFile=/var/tmp/metabase.pid
WorkingDirectory=/usr/local/bin/metabase/
User=ubuntu
Group=ubuntu

# Run command here
ExecStart=/usr/bin/java -jar -Xmx512m /usr/local/bin/metabase/metabase.jar

Restart=on-failure
RestartSec=30
PrivateTmp=true

StandardOutput=metabase
StandardError=metabase_err
SyslogIdentifier=metabase

[Install]
WantedBy=multi-user.target
```

**Explanation:**
- **[Unit]:** Provides metadata about the service, including its description.
- **[Service]:** Defines how the service runs, including the command to start it (`ExecStart`), user and group under which it runs, and how to handle failures.
- **[Install]:** Configures how the service should be installed and started.

### **Step 4: Manage Your Service**

Once you’ve created and saved your service file, you need to reload systemd to recognize your new service and then manage it using `systemctl`.

**Start the Service:**
```bash
sudo systemctl start whatever_you_want.service
```

**Enable Auto-Start at Boot:**
```bash
sudo systemctl enable whatever_you_want.service
```

**Disable Auto-Start:**
```bash
sudo systemctl disable whatever_you_want.service
```

**Stop the Service:**
```bash
sudo systemctl stop whatever_you_want.service
```

**Restart the Service:**
```bash
sudo systemctl restart whatever_you_want.service
```

**View Service Logs:**
```bash
journalctl -e -u whatever_you_want.service
```

This command shows the most recent log entries for your service, which is useful for troubleshooting and monitoring.

### **Conclusion**

Creating and managing a systemd service on Linux is a powerful way to handle background tasks and system daemons. By following these steps, you can easily set up your services to run automatically at boot and manage them with simple commands. Whether you’re running a web application, a background process, or any other service, systemd provides the tools you need to keep things running smoothly and efficiently.