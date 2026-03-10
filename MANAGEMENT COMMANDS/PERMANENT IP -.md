# Network 🌐 Configuration

[](https://github.com/InfoSecWarrior/Linux-Essentials/blob/main/Network-Configuration-Basic.md#network--configuration)

## Network Configuration Basics

[](https://github.com/InfoSecWarrior/Linux-Essentials/blob/main/Network-Configuration-Basic.md#network-configuration-basics)

The following commands help manage and verify network settings:

### Viewing Network Information

[](https://github.com/InfoSecWarrior/Linux-Essentials/blob/main/Network-Configuration-Basic.md#viewing-network-information)

- `nmtui` - Opens a text-based user interface for configuring network connections.
- `hostname` - Displays or sets the system's hostname.
- `hostname -i` - Shows the system's IP address associated with the hostname.
- `hostname -I` - Lists all assigned IP addresses.
- `ip addr show` - Displays details of all network interfaces.
- `ifconfig` - Displays network interface configurations.
- `ip ad` - Similar to `ip addr show`, lists network interfaces and IPs.

### Checking Public IP Address

[](https://github.com/InfoSecWarrior/Linux-Essentials/blob/main/Network-Configuration-Basic.md#checking-public-ip-address)

- `curl -4 ifconfig.me` - Retrieves the system's external IPv4 address.
- `curl -4 icanhazip.com` - Alternative way to check public IP.
- `cat /etc/resolv.conf` - Shows DNS resolver settings.
- `route -n` - Displays the kernel routing table.
- `ip route` - Displays and manipulates the system's routing table (part of the `iproute2` package).

## Configuring Static IP Address

[](https://github.com/InfoSecWarrior/Linux-Essentials/blob/main/Network-Configuration-Basic.md#configuring-static-ip-address)

To configure a static IP address using NetworkManager:

1. Navigate to the NetworkManager system connections directory:
    
    ```shell
    cd /etc/NetworkManager/system-connections/
    ```
    
2. Open the connection file with Vim:
    
    ```shell
    vim enp0s3.nmconnection
    ```
    
3. Modify the configuration file with a static IP:
    
    ```ini
    [connection]
    id=enp0s3
    uuid=668303d1-c039-30c9-82bf-0f3527891838
    type=ethernet
    autoconnect-priority=-999
    interface-name=enp0s3
    timestamp=1726179824
    
    [ethernet]
    
    [ipv4]
    address1=192.168.1.31/24,192.168.1.1
    dns=8.8.8.8;
    method=manual
    
    [ipv6]
    addr-gen-mode=eui64
    method=manual
    
    [proxy]
    ```
    
    - `address1=192.168.1.34/24` - Assigns `192.168.1.34` as the static IP with a subnet mask of `255.255.255.0` (`/24`) and sets the gateway to `192.168.1.1`.
    - `dns=8.8.8.8;8.8.4.4;` - Uses Google's public DNS.
    - `method=manual` - Configures IPv4 manually.

## Configuring Dynamic IP (DHCP)

[](https://github.com/InfoSecWarrior/Linux-Essentials/blob/main/Network-Configuration-Basic.md#configuring-dynamic-ip-dhcp)

4. Navigate to the NetworkManager system connections directory:
    
    ```shell
    cd /etc/NetworkManager/system-connections/
    ```
    
5. Open the connection file with Vim:
    
    ```shell
    vim enp0s3.nmconnection
    ```
    
6. Modify the configuration file:
    
    ```ini
    [connection]
    id=enp0s3
    uuid=a32babco-fbae-3689-b858-40d62bd6d445
    type=ethernet
    autoconnect-priority=-999
    interface-name=enp0s3
    timestamp=1739793171
    
    [ethernet]
    
    [ipv4]
    method=auto
    
    [ipv6]
    addr-gen-mode=eui64
    method=disabled
    
    [proxy]
    ```
    
    - `method=auto` enables DHCP, allowing the system to obtain an IP address automatically.

## Assigning Multiple IPs to a Single Network Interface

[](https://github.com/InfoSecWarrior/Linux-Essentials/blob/main/Network-Configuration-Basic.md#assigning-multiple-ips-to-a-single-network-interface)

To assign multiple IP addresses to a single interface (`enp0s3`):

7. Navigate to the system connections directory:
    
    ```shell
    cd /etc/NetworkManager/system-connections/
    ```
    
8. Edit the connection file:
    
    ```shell
    vim enp0s3.nmconnection
    ```
    
9. Modify the configuration file:
    
    ```ini
    [connection]
    id=enp0s3
    uuid=a32babco-fbae-3689-b858-40d62bd6d445
    type=ethernet
    autoconnect-priority=-999
    interface-name=enp0s3
    timestamp=1739793171
    
    [ethernet]
    
    [ipv4]
    address1=192.168.1.34/24
    address2=192.168.1.35/24
    address3=192.168.1.36/24
    address4=192.168.1.37/24
    dns=8.8.8.8,8.8.4.4;
    gateway=192.168.1.1
    method=manual
    
    [ipv6]
    addr-gen-mode=eui64
    method=disabled
    
    [proxy]
    ```
    
    - Each additional IP (`.32, .33, .34`) is assigned to the same interface.




f2 ya f3 se dusre cmd pe aa jao
# nmtui-
`nmtui` stands for **Network Manager Text User Interface**. It's a command-line tool used for managing network connections on Linux systems. It provides a simple text-based interface for configuring network settings like Wi-Fi, Ethernet, VPNs, and more, without needing to manually edit configuration files or use more complex commands.

When you run `nmtui` in the terminal, it opens an interactive menu where you can:

10. **Edit connections** – Add, modify, or delete network connections.
11. **Activate a connection** – Enable or disable network interfaces.
12. **Set system hostname** – Change the system’s hostname.

It’s especially useful on servers or systems without a graphical interface, providing a user-friendly way to manage network settings. To start it, just type `nmtui'
# steps to change ip by nmtui-
To change your IP address using the `nmtui` (NetworkManager Text User Interface) command, follow these steps:

13. **Open a terminal** and type the following command to launch `nmtui`:
    
    bash
    
    Copy
    
    `sudo nmtui`
    
14. **Select "Edit a connection"**: In the menu that appears, use the arrow keys to highlight and select **Edit a connection**. Then press **Enter**.
    
15. **Choose the network interface**: You will see a list of available network connections. Use the arrow keys to select the network interface for which you want to change the IP (e.g., `Wired connection 1`, `Wi-Fi`, etc.). Then press **Enter**.
    
16. **Edit the IP settings**:
    
    - For a **static IP** (manual IP), use the arrow keys to navigate to **IPv4 Settings** (or IPv6 Settings if you’re configuring IPv6). Press **Enter**.
    - Set the **Method** to **Manual**.
    - Enter the **IP address**, **netmask** (e.g., `255.255.255.0`), and **gateway** information in the appropriate fields.
    - You can also set DNS servers if necessary.
17. **Save your changes**:
    
    - After entering your desired settings, use the **Tab** key to move to **OK** and press **Enter** to save the changes.
18. **Activate the connection**:
    
    - You can now go back to the main menu by selecting **Back** and pressing **Enter**.
    - In the main menu, select **Activate a connection**.
    - Choose the connection you just modified, and press **Enter** to activate it.
19. **Exit `nmtui`**:
    
    - Once you have activated the connection, select **Quit** in the main menu to exit `nmtui`.

Your new IP address settings should now be applied. You can verify them by using the `ip a` or `ifconfig` command in the terminal.
multiple ip de ke bhi unnse ssh bana sakte hai


#hostname - server name
#hostname -i = for ip
#hostname -I =for all active and deactive ip
/etc/hostname me jake server name set kar sakte hai
for gateway check -
cat /etc/resolv.conf
route -n

#ifconfig -a = for hidden interface

https://icanhazip.com/ for private ip of a system 
 
 by command 
 curl  -4 https://icanhazip.com/ retrives the system  external ip v4 
 ip route - konsa ip kis gateway or network ko route kar rha hai
# configure the static ip=
cd /etc/networkmanager/system-connections/
then interface milega ab usse vim karke edit kar lo
# by dynamic ip=
cd /etc/networkmanager/system-connections/
then interface milega ab usse vim karke edit kar lo
manual ko automatic kar lo 
toh dhcp se ip lega
# in debian by file=
vim /etc/network/interface




\