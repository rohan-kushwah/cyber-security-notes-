Service Management in linux   
 #key Points of a Linux Service:  - **Background Process**: Runs in the background without user intervention, unlike typical applications.
  - **System Management**: Manages essential functions like networking, storage, logging, etc.
  - **Controlled by Init Systems**: Managed by an init system like systemd, upstart, or SysVinit, which controls the starting, stopping, and restarting of services. 
  - **Configuration and Logs**: Usually have configuration files in `/etc` and log files in `/var/log`.
  -  ## Managing Services with SysVinit  SysVinit is an older service management system used in Debian-based and some older Linux distributions. Here are the commands for managing services using `/etc/init.d/`: 
  - ### Checking Available Init Scripts  
  - ```bash # cd /etc/init.d/ 
  - ### ls -lh /etc/init.d/``

### To View an Init Script

bash

Copy

`# cat /etc/init.d/service-name # e.g. # cat /etc/init.d/apache2`

### Check the Status of a Service

bash

Copy

`# /etc/init.d/apache2 status`

### Start or Stop a Service

bash

Copy

`# /etc/init.d/apache2 start   # To start # /etc/init.d/apache2 stop    # To stop`

### Restart or Stop a Service

bash

Copy

`# /etc/init.d/apache2 restart  # To restart`

### Reload a Service

bash

Copy

`# /etc/init.d/apache2 force-reload`

---

## Managing Services in Latest Linux Systems

Managing services in Linux is essential for system administration. Below are key commands to start, stop, restart, enable, disable, and monitor services using `systemctl` and `service`.

---
netstat -nltup network services dekhne ke lie
### Managing Services with `service` Command (Legacy Systems)

For older Linux distributions that use init.d or SysVinit instead of systemd:

#### Check the Status of a Service

bash

Copy

`# service <service_name> status # e.g. # service apache2 status`

#### Start a Service

bash

Copy

`# service <service_name> start # e.g. # service apache2 start`

#### Stop a Service

bash

Copy

`# service <service_name> stop # e.g. # service apache2 stop`

#### Restart a Service

bash

Copy

`# service <service_name> restart # e.g. # service apache2 restart`

#### Reload a Service (Without Restart)

bash

Copy

`# service <service_name> reload # e.g. # service apache2 reload`

---

### Managing Services with `systemctl`

For modern Linux systems that use `systemd`:

#### Install a Package (e.g., Apache)

bash

Copy

`# yum install httpd*`

#### Checking Service Status

bash

Copy

`# systemctl status <service_name>`

#### Start and Stop Services

bash

Copy

`# systemctl start <service_name> # systemctl stop <service_name>`

#### Restart a Service

bash

Copy

`# systemctl restart <service_name>`

#### Reload a Service

bash

Copy

`# systemctl reload <service_name>`

#### Enable a Service to Start at Boot

bash

Copy

`# systemctl enable <service_name>`

#### Disable a Service from Starting at Boot

bash

Copy

`# systemctl disable <service_name>`

#### Check if a Service is Enabled or Disabled at Startup

bash

Copy

`# systemctl is-enabled <service_name>`

#### List All Active Services

bash

Copy

`# systemctl list-units --type=service --state=running`

#### List Failed Services

bash

Copy

`# systemctl list-units --type=service --state=failed`

#### Mask a Service (Prevent It from Running)

bash

Copy

`# systemctl mask <service_name>`

#### Unmask a Service (Allow It to Run Again)

bash

Copy

`# systemctl unmask <service_name>`

#### Show Logs for a Specific Service

bash

Copy

`# journalctl -u <service_name> # e.g. # journalctl -u httpd.service`

#### Display Logs for a Service from the Last Hour

bash

Copy

`# journalctl -u httpd.service --since "1 hour ago"`

#### View Live Logs (Useful for Debugging)

bash

Copy

`# journalctl -u httpd.service -f`

---

## System Management Commands

- **Reboot the System**

bash

Copy

`# systemctl reboot`

- **Shut Down the System**

bash

Copy

`# systemctl poweroff`

- **Suspend the System**

bash

Copy

`# systemctl suspend`

- **Hibernate the System**
- The command `systemctl hibernate` is used in Linux-based operating systems to put the system into hibernation mode. In hibernation, the system saves the contents of the RAM to a swap space (or swap file) on the disk, then powers off the compute

bash

Copy

`# systemctl hibernate`

---

## SysVinit vs Systemd (`systemctl`)

Modern Linux distributions (Debian 8+, Ubuntu 15.04+, CentOS 7+, RHEL 7+) use `systemd`. Instead of `/etc/init.d/`, you should use the following commands:

- **Start a Service**

bash

Copy

`# systemctl start apache2`

- **Stop a Service**

bash

Copy

`# systemctl stop apache2`

- **Restart a Service**

bash

Copy

`# systemctl restart apache2`

- **Check the Status of a Service**

bash

Copy

`# systemctl status apache2`

- **Enable a Service to Start at Boot**

bash

Copy

`# systemctl enable apache2`

- **Disable a Service from Starting at Boot**

bash

Copy

`# systemctl disable apache2`


Summary of Service Management Commands

|Command|Purpose|
|---|---|
|`systemctl status <service>`|Check service status|
|`systemctl start <service>`|Start a service|
|`systemctl stop <service>`|Stop a service|
|`systemctl restart <service>`|Restart a service|
|`systemctl reload <service>`|Reload configuration without restart|
|`systemctl enable <service>`|Enable service at boot|
|`systemctl disable <service>`|Disable service at boot|
|`systemctl list-units --type=service`|List active services|
|`systemctl list-units --state=failed`|List failed services|
|`systemctl mask <service>`|Completely disable a service|
|`systemctl unmask <service>`|Re-enable a masked service|
|`journalctl -u <service>`|View service logs|
|`systemctl --failed`|List all failed services|
Creating custom services in Linux typically involves creating a service that will be managed by a service manager like `systemd`, which is the most common init system in modern Linux distributions. Here's a step-by-step guide on how to create a custom service using `systemd`:

### 1. **Create a Script for the Service**

First, you need a script or program that your service will run. This can be any executable, shell script, or command.

For example, let’s create a simple bash script `/usr/local/bin/my_custom_service.sh`:

bash

Copy

`#!/bin/bash echo "My custom service is running!" > /tmp/custom_service.log`

Make sure it’s executable:

bash

Copy

`sudo chmod +x /usr/local/bin/my_custom_service.sh`

### 2. **Create a Systemd Service Unit File**

Systemd uses service unit files to define how services should be managed. These files are usually located in `/etc/systemd/system/`.

Create a new service file for your custom service:

bash

Copy

`sudo nano /etc/systemd/system/my_custom_service.service`

Add the following content to the service file:

ini

Copy

`[Unit] Description=My Custom Service After=network.target  [Service] ExecStart=/usr/local/bin/my_custom_service.sh Restart=always User=nobody  [Install] WantedBy=multi-user.target`

### Breakdown of the Service File:

- **[Unit]**
    
    - `Description`: A short description of your service.
    - `After`: Specifies the order in which your service should start. In this case, it's after the network service is up.
- **[Service]**
    
    - `ExecStart`: The command or script to run when the service starts.
    - `Restart`: Defines the service’s restart behavior (e.g., `always` means it restarts if it crashes).
    - `User`: The user under which the service will run. You can change `nobody` to another user if needed.
- **[Install]**
    
    - `WantedBy`: Specifies when the service should be started (in this case, it is part of the `multi-user` target, meaning it will start after the system reaches the multi-user state).

### 3. **Reload Systemd and Enable the Service**

After creating your service file, you need to reload the systemd manager to recognize the new service and then enable and start it.

bash

Copy

`# Reload systemd to recognize the new service sudo systemctl daemon-reload  # Enable the service to start on boot sudo systemctl enable my_custom_service.service  # Start the service sudo systemctl start my_custom_service.service`

### 4. **Check the Status of the Service**

To verify that the service is running correctly, use:

bash

Copy

`sudo systemctl status my_custom_service.service`

This will show the status, logs, and any potential issues with your service.

### 5. **Stop and Disable the Service (If Needed)**

If you need to stop the service or disable it from starting at boot, use these commands:

bash

Copy

`# Stop the service sudo systemctl stop my_custom_service.service  # Disable the service from starting on boot sudo systemctl disable my_custom_service.service`

### 6. **View Logs**

To check the output or logs generated by the service, you can use `journalctl`:

bash

Copy

`sudo journalctl -u my_custom_service.service`

This will show the logs specific to your custom service.