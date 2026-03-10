# **Network Monitoring with `netstat` and `ss`**

[](https://github.com/InfoSecWarrior/Linux-Essentials/blob/a7dc5f43d9f129b038d6ecfe9be724d0206e369f/Network_Monitoring_with_netstat_and_ss.md#network-monitoring-with-netstat-and-ss)

## Installation

[](https://github.com/InfoSecWarrior/Linux-Essentials/blob/a7dc5f43d9f129b038d6ecfe9be724d0206e369f/Network_Monitoring_with_netstat_and_ss.md#installation)

To install the required tools, use:

```shell
yum install net-tools -y

```

## **Using `netstat`**

[](https://github.com/InfoSecWarrior/Linux-Essentials/blob/a7dc5f43d9f129b038d6ecfe9be724d0206e369f/Network_Monitoring_with_netstat_and_ss.md#using-netstat)

The netstat command is used to ==display network status, troubleshoot, and monitor network connections==. It can also be used to configure networks. 

What it displays 

- Active TCP connections
- Ports that the computer is listening on
- Ethernet statistics
- IP routing table

- IPv4 and IPv6 statistics

- Routing table information

- Interface information

- Status of TCP and UDP endpoints

### **Basic Usage**

[](https://github.com/InfoSecWarrior/Linux-Essentials/blob/a7dc5f43d9f129b038d6ecfe9be724d0206e369f/Network_Monitoring_with_netstat_and_ss.md#basic-usage)

- Display active connections:
    
    ```shell
    netstat
    
    ```
    
- Show all listening and non-listening (active) connections:
    
    ```shell
    netstat -a
    
    ```
    
- Display numeric addresses instead of resolving hostnames:
    
    ```shell
    netstat -na
    
    ```
    
- Filter by protocol:
    
    - Show only TCP connections:
        
        ```shell
        netstat -t
        
        ```
        
    - Show only UDP connections:
        
        ```shell
        netstat -u
        
        ```
        
    - Show both TCP and UDP connections:
        
        ```shell
        netstat -tu
        
        ```
        
- Display TCP and UDP connections in numeric format:
    
    ```shell
    netstat -tun
    
    ```
    
- Show listening TCP and UDP ports:
    
    ```shell
    netstat -ltun
    
    ```
    
- Display all listening services, numeric IPs, and process details (more accurate):
    
    ```shell
    netstat -nltup
    netstat -natup
    
    ```
    

## **Using `ss`**

[](https://github.com/InfoSecWarrior/Linux-Essentials/blob/a7dc5f43d9f129b038d6ecfe9be724d0206e369f/Network_Monitoring_with_netstat_and_ss.md#using-ss)

The `ss` command is a faster alternative to `netstat` for displaying socket statistics.

### **Basic Usage**

[](https://github.com/InfoSecWarrior/Linux-Essentials/blob/a7dc5f43d9f129b038d6ecfe9be724d0206e369f/Network_Monitoring_with_netstat_and_ss.md#basic-usage-1)

- Display active connections:
    
    ```shell
    ss
    
    ```
    
- Show all listening sockets:
    
    ```shell
    ss -l
    
    ```
    
- Display TCP connections:
    
    ```shell
    ss -t
    
    ```
    
- Display UDP connections:
    
    ```shell
    ss -u
    
    ```
    
- Display listening TCP and UDP ports:
    
    ```shell
    ss -ltun
    
    ```
    
- Display detailed process information for each socket:
    
    ```shell
    ss -tp
    
    ```
    

## **Comparison: `netstat` vs. `ss`**

[](https://github.com/InfoSecWarrior/Linux-Essentials/blob/a7dc5f43d9f129b038d6ecfe9be724d0206e369f/Network_Monitoring_with_netstat_and_ss.md#comparison-netstat-vs-ss)

|Feature|`netstat`|`ss`|
|---|---|---|
|Speed|Slower|Faster|
|Availability|Requires `net-tools` package|Pre-installed in modern Linux|
|Features|Comprehensive but outdated|More detailed and efficient|
|Performance|Higher resource usage|Lower resource usage|
|Filtering|Basic filtering|Advanced filtering|
|IPv6 Support|Limited support|Full support|

## **Conclusion**

[](https://github.com/InfoSecWarrior/Linux-Essentials/blob/a7dc5f43d9f129b038d6ecfe9be724d0206e369f/Network_Monitoring_with_netstat_and_ss.md#conclusion)

`netstat` and `ss` are essential tools for monitoring and troubleshooting network connections in CentOS. While `netstat` is widely used, `ss` is a faster and more efficient alternative for analyzing socket connections. Both tools provide detailed insights into active connections, routing, and protocol-specific statistics, making them valuable for network administrators and system engineers.






netstat - Network Monitoring: netstat and ss Commands
Installation (if not available)
# yum install net-tools         # RHEL/CentOS
# apt install net-tools         # Debian/Ubuntu