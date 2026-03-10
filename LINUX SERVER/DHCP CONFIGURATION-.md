DHCP  DISTRIBUTE IP TO THE DEVICES IN THE NETWORK 
# CONFIGURATION-
**DHCP Server**  
**DHCP Server Configuration Guide**

This document provides a step-by-step guide to install, configure, and manage a DHCP server on a Linux-based system. The commands are documented with explanations for better clarity.

### 1. Check Available Repositories

List all available repositories to ensure that the DHCP server package is available:

bash

CopyEdit

`yum repolist all`

### 2. Verify if DHCP Package is Installed

Check if the DHCP package is installed:

bash

CopyEdit

`rpm -qa | grep dhcp`

### 3. Install DHCP Server

Install the DHCP server package:

bash

CopyEdit

`yum install dhcp-server.x86_64`

### 4. Verify Installation

Check if the DHCP package and the DHCP server are installed:

bash

CopyEdit

`rpm -qa | grep dhcp rpm -qa | grep dhcp-server`
### 5. Get Detailed Information About DHCP Server

Display detailed information about the installed DHCP server package:

bash

CopyEdit

`rpm -qi dhcp-server`

### 6. List Documentation Files for DHCP Server

List the documentation files included with the DHCP server:

bash

CopyEdit

`rpm -qd dhcp-server`

### 7. List Configuration Files for DHCP Server

List the configuration files included with the DHCP server:

bash

CopyEdit

`rpm -qc dhcp-server`

### 8. List All Files Installed by DHCP Server

List all files installed by the DHCP server package:

bash

CopyEdit

`rpm -ql dhcp-server`

### 9. Edit DHCP Configuration File

Open the DHCP configuration file for editing:

bash

CopyEdit

`vim /etc/dhcp/dhcpd.conf`
![[Pasted image 20250311130355.png]]

### 10. Reference Example Configuration

Open the example configuration file for reference:

bash

CopyEdit

`vim /usr/share/doc/dhcp-server/dhcpd.conf.example`

### 11. Start and Check DHCP Service

#### Check Service Status:

bash

CopyEdit

`systemctl status dhcpd`

#### Start Service:

bash

CopyEdit

`systemctl start dhcpd`

#### Restart Service:

bash

CopyEdit

`systemctl restart dhcpd`

#### Enable Service to Start at Boot:

bash

CopyEdit

`systemctl enable dhcpd.service`
### 2. Check Open Ports

Check which ports are open and listening:

bash

CopyEdit

`netstat -nltup`

Check if DHCP ports are open:

bash

CopyEdit

`netstat -nltup | grep dhcp`

### 13. Update Firewall Rules (Firewalld)

#### Open DHCP Port in Firewall:

Allow DHCP traffic through `firewalld`:

bash

CopyEdit

`firewall-cmd --add-port=67/udp --permanent`

#### Reload Firewall:

Reload the firewall to apply changes:

bash

CopyEdit

`firewall-cmd --reload`

#### Verify Firewall Rules:

Check if the DHCP port is open:

bash

CopyEdit

`firewall-cmd --list-ports`

Alternatively, check the detailed firewall configuration:

bash

CopyEdit

`firewall-cmd --list-all`

EXAMPLE--
      THEN AB JAB DHCP  IP DEGA TOH WO IS FILE ME HOGI
      CAT /VAR/LIB/dhcpd/dhcpd.lease
      ab clients ko ip dila do


# exclusion range--
## **Configure DHCP Exclusion Range**

You can define an exclusion range in the DHCP configuration file to prevent certain IP addresses within the subnet from being assigned to clients.

### **Step 1: Edit DHCP Configuration File**

Open the DHCP configuration file for editing:

bash

CopyEdit

`vim /etc/dhcp/dhcpd.conf`

### **Step 2: Example Configuration for Exclusion Range**

You can define multiple `range` statements to exclude specific IP addresses within the subnet.

#### **Example 1: Exclude** `192.168.1.211` **to** `192.168.1.230`

This example configures an exclusion between `192.168.1.211` and `192.168.1.230`:

bash

CopyEdit

# ````
```DHCP Server Configuration File.
# See /usr/share/doc/dhcp*/dhcpd.conf.example
# See dhcpd.conf(5) man page

authoritative;

# Specify network address and subnet mask
subnet 192.168.1.0 netmask 255.255.255.0 {
    
    # Specify the range of lease IP address
    range 192.168.1.0 192.168.1.210;
    range 192.168.1.231 192.168.1.250;

    # Specify default gateway
    option routers 192.168.1.1;

    # DNS servers for name resolution
    option domain-name-servers 8.8.8.8, 8.8.4.4;

    # Specify broadcast address
    option broadcast-address 192.168.1.255;

    # Default lease time
    default-lease-time 600;

    # Max lease time
    max-lease-time 7200;
}

```

Step 3: Restart DHCP Service

After making changes to the configuration file, restart the DHCP service:

# systemctl restart dhcpd

✅ Explanation:

- **Excluded IP Addresses:** Any addresses between the defined `range` statements will be excluded from assignment.
  - **Example 1:** IPs **192.168.1.211** to **192.168.1.230** are excluded.
  - **Example 2:** IPs **192.168.1.51** to **192.168.1.60** and **192.168.1.211** to **192.168.1.230** are excluded.
- This allows better control over IP allocation and avoids conflicts with static IP addresses or reserved devices.

# RESERVE IP==
  BY MAC RESERVE KARENGE
  Step 1: Edit DHCP Configuration File

Open the DHCP configuration file for editing:

vim /etc/dhcp/dhcpd.conf

Step 2: Example Configuration for IP Reservation

Add a host block within the subnet configuration to reserve an IP address for a specific MAC address.
Example 1: Reserve IP Address for a Single Client

This example reserves IP 192.168.1.201 for the client with MAC address 08:00:27:5d:b8:bc:

```
```authoritative;

# Specify network address and subnet mask
subnet 192.168.1.0 netmask 255.255.255.0 {
    # Specify the range of lease IP address
    range 192.168.1.50 192.168.1.50;
    range 192.168.1.61 192.168.1.210;
    range 192.168.1.231 192.168.1.250;

    # Specify default gateway
    option routers 192.168.1.1;

    # DNS servers for name resolution
    option domain-name-servers 8.8.8.8, 8.8.4.4;

    # Specify broadcast address
    option broadcast-address 192.168.1.255;

    # Default lease time
    default-lease-time 600;

    # Max lease time
    max-lease-time 7200;
}

# Reserve IP address for specific client
host win7 {
    # MAC address of client for fixed IP
    hardware ethernet 08:00:27:5d:b8:bc;
    # Reserved IP address
    fixed-address 192.168.1.201;
}
```

Step 3: Restart DHCP Service

After modifying the configuration file, restart the DHCP service:

#```
systemctl restart dhcpd


✅ Explanation:

- **hardware ethernet** – The MAC address of the client device.
- **fixed-address** – The IP address to assign to the specified MAC address.
- **Effect**: When a client with a matching MAC address requests an IP address, the DHCP server will assign the reserved IP.
- Reserved IP addresses should be **outside the DHCP pool range** to avoid conflicts.
- Ensure that the reserved IP addresses are not part of the dynamic IP address range.


# ALLOW LIST & DENY LIST==

PARTICULAR MAC ADDRESS KO IP  NHI DILWANA NHI HAI
### **Example 1: Deny a Single Client**

This example denies the client with MAC address **08:00:27:ab:46:cf** from getting an IP address:

plaintext

CopyEdit
# ```
```DHCP Server Configuration file.
# see /usr/share/doc/dhcp*/dhcpd.conf.example
# see dhcpd.conf(5) man page

authoritative;

# Specify network address and subnet mask
subnet 192.168.1.0 netmask 255.255.255.0 {
    # Specify the range of lease IP address
    range 192.168.1.50 192.168.1.50;
    range 192.168.1.61 192.168.1.210;
    range 192.168.1.231 192.168.1.250;

    # Specify default gateway
    option routers 192.168.1.1;

    # DNS servers for name resolution
    option domain-name-servers 8.8.8.8, 8.8.4.4;

    # Specify broadcast address
    option broadcast-address 192.168.1.255;

    # Default lease time
    default-lease-time 600;

    # Max lease time
    max-lease-time 7200;
}

# Deny specific client from obtaining IP
host win11 {
    hardware ethernet 08:00:27:ab:46:cf;
    deny booting;
}

```

THEN 
```
SYSTEMCTL  RESTART DHCPD
```

# ALLOW LIST--
### **Whitelist/Allow List Clients**

You can configure a DHCP server to only provide IP addresses to specific known clients by using the `deny unknown-clients` directive. This restricts DHCP access to only whitelisted devices.

### **Step 1: Edit DHCP Configuration File**

Open the DHCP configuration file for editing:

shell

CopyEdit

`# vim /etc/dhcp/dhcpd.conf`

### **Step 2: Example Configuration for Whitelisting Clients**

You can whitelist specific clients by adding their MAC addresses in `host` blocks and setting `deny unknown-clients` within the `subnet` block.

### **Example: Allow Only Specific Clients**

This example allows only the clients with MAC addresses `08:00:27:5d:b8:bc` and `08:00:27:fd:a8:e3` to receive an IP address.
# ```
```DHCP Server Configuration file.
#    see /usr/share/doc/dhcp-server/dhcpd.conf.example
#    see dhcpd.conf(5) man page

authoritative;

# Specify network address and subnet mask
subnet 192.168.1.0 netmask 255.255.255.0 {
    # Specify the range of lease IP address
    range 192.168.1.50 192.168.1.50;
    range 192.168.1.61 192.168.1.210;
    range 192.168.1.231 192.168.1.250;

    # Specify default gateway
    option routers 192.168.1.1;

    # DNS servers for name resolution
    option domain-name-servers 8.8.8.8, 8.8.4.4;

    # Specify broadcast address
    option broadcast-address 192.168.1.255;

    # Default lease time
    default-lease-time 600;

    # Max lease time
    max-lease-time 7200;

    # Deny unknown clients from getting an IP address
    deny unknown-clients;
}

host win11 { hardware ethernet 08:00:27:d6:86:94; }

```

THEN RESTART SERVICES--\
systemctl restart dhcpd



#  giving ip to known and unknown clients-
known jiska mac wha define ho
# **🔄 DHCP Configuration: Different Pools for Known & Unknown Clients**

You can configure your **DHCP server to assign different IP address pools** based on whether the client is known (listed in the configuration) or unknown.

---

## **1️⃣ Edit the DHCP Configuration File**

📝 Open the DHCP configuration file for editing:

```shell
vim /etc/dhcp/dhcpd.conf
```

---

## **2️⃣ Example Configuration for Differentiating Known and Unknown Clients**

To allow only **specific clients** in one IP range and **assign another range to unknown clients**, use separate `pool` directives.

### **Example: Assign Separate Pools**

```shell
authoritative;

# Specify network address and subnet mask
subnet 192.168.1.0 netmask 255.255.255.0 {

    # Specify default gateway
    option routers 192.168.1.1;

    # DNS servers for name resolution
    option domain-name-servers 8.8.8.8, 8.8.4.4;

    # Specify broadcast address
    option broadcast-address 192.168.1.255;

    # Default lease time (in seconds)
    default-lease-time 600;

    # Maximum lease time (in seconds)
    max-lease-time 7200;

    # Pool for known clients
    pool {
        range 192.168.1.150 192.168.1.200;
        allow known-clients;
    }

    # Pool for unknown clients
    pool {
        range 192.168.1.201 192.168.1.250;
        allow unknown-clients;
    }
}

# Define known clients
host win11-1 {
    hardware ethernet 08:00:27:d6:86:94;
    fixed-address 192.168.1.151;
}

host win11-2 {
    hardware ethernet 08:00:27:fd:a8:e3;
    fixed-address 192.168.1.152;
}
```

---

## **3️⃣ Restart DHCP Service**

After modifying the configuration file, restart the DHCP service for changes to take effect:

```shell
systemctl restart dhcpd
```

---

## **🛠️ Explanation**

✅ `allow known-clients;` → **Assigns IPs from the specified pool to recognized devices** (those with defined MAC addresses).  
✅ `allow unknown-clients;` → **Assigns IPs from another pool to unrecognized devices** (any device not listed as a host).  
✅ `host` directive → **Defines specific MAC addresses as known clients** and optionally assigns a fixed IP.

### **⚠️ Why Use Separate Pools?**

🔹 **Enhanced network security** by controlling DHCP assignments.  
🔹 **Prioritization of trusted devices** with a different IP range.  
🔹 **Prevention of IP conflicts** by assigning fixed IPs to known devices.