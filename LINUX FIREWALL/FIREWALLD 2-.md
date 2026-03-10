| Command                       | Description                         |
| ----------------------------- | ----------------------------------- |
| `systemctl start firewalld`   | Start Firewalld                     |
| `systemctl stop firewalld`    | Stop Firewalld                      |
| `systemctl restart firewalld` | Restart Firewalld                   |
| `systemctl enable firewalld`  | Enable Firewalld at system startup  |
| `systemctl disable firewalld` | Disable Firewalld at system startup |
| `systemctl status firewalld`  | Check Firewalld status              |
| `firewall-cmd --state`        | Check if Firewalld is running       |

### 🔥 **Zone Management Commands**

|Command|Description|
|---|---|
|`firewall-cmd --get-zones`|List available zones|
|`firewall-cmd --get-default-zone`|Get the default zone|
|`firewall-cmd --set-default-zone=public`|Change the default zone|

---

### 🔥 **Interface and Service Management**
koi bhi service allow karoge toh jo port pe wo service chalegi use by default allow kar degaa


| Command                                                       | Description             |
| ------------------------------------------------------------- | ----------------------- |
| `firewall-cmd --get-active-zones`                             | Show active zones       |
| `firewall-cmd --zone=public --add-interface=eth0`             | Add `eth0` to a zone    |
| `firewall-cmd --get-services`                                 | List available services |
| `firewall-cmd --zone=public --add-service=ssh --permanent`    | Allow SSH service       |
| `firewall-cmd --zone=public --remove-service=ssh --permanent` | Remove SSH service      |
| selinux check /etc/sysconfig/selinux                          | enable hai ya disable   |

---

### 🔥 **Advanced Rich Rules**

| Command                                                                                                                   | Description                                                                                |
| ------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| `firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.1.100" reject' --permanent`       | Block IP `192.168.1.100`                                                                   |
| `firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.1.100" accept' --permanent`       | Allow IP `192.168.1.100`                                                                   |
| firewall-cmd --zone=public --add-rich-rule'rule family="ipv4" source address="192.168.1.100/20" accept' --permament       | pura network bandh                                                                         |
| firewall -cmd --permanent --add-rich-rule='rule family="ipv4"source address="ip" port protocol="tcp" port="22" drop'      | kisi ek particular ip ke liye wo service na chale                                          |
| firewall -cmd --permanent --add-rich-rule='rule family="ipv4"destination address="ip" port protocol="tcp" port="22" drop' | ab jis server se ssh lena hai us server ke particular ip se host ka connection nhi baneega |

---

### 🔥 **Reset and Reload Firewalld**

|Command|Description|
|---|---|
|`firewall-cmd --complete-reload`|Completely reload Firewalld (removes temporary rules)|
|`firewall-cmd --permanent --remove-service=http`|Remove HTTP service permanently|
|`firewall-cmd --runtime-to-permanent`|Save runtime changes permanently|


# for remove rules-
```
firewall-cmd --permanent --remove-port=21/tcp
```


*firewall -cmd --permanent --remove-rich-rule='rule family="ipv4"destination address="ip" port protocol="tcp" port="22" drop'*

''' 
pura ek zone ka data hatana ho toh zone file hata do
'''
 /etc/firewalld/zones
 then zone file uda do

file reboot me firse bana dega
 nhi banai toh


firerwall -cmd --runtime-to-permanent


then 
firewal l-cmd --complete -reload



# OUTBOUND TRAFFIC-
`firewalld` is a dynamic firewall manager used in Linux systems, typically on distributions like **CentOS**, **RHEL**, and **Fedora**. It uses zones to manage firewall rules and supports both IPv4 and IPv6. When managing outbound traffic with `firewalld`, you can configure rules to control the traffic leaving your system (outbound traffic) based on your needs.

Here’s how you can manage **outbound traffic** with `firewalld`:

### 1. **Allowing Outbound Traffic**

By default, `firewalld` allows all outbound traffic, but you can explicitly define rules to control it.

- **Allow All Outbound Traffic (default behavior)**: Normally, all outbound traffic is allowed unless restricted by specific rules.
    
    bash
    
    Copy
    
    `sudo firewall-cmd --zone=public --add-masquerade --permanent`
    
    The above command adds masquerading, which helps in NAT (Network Address Translation) and allows the system to make outbound connections. This is usually not necessary unless you are doing some form of network address translation.
    

### 2. **Restricting Outbound Traffic**

If you want to restrict outbound traffic to specific services or ports, here’s how you can proceed:

- **Deny Outbound Traffic on a Specific Port** (e.g., block outbound HTTP traffic on port 80):
    
    bash
    
    Copy
    
    `sudo firewall-cmd --zone=public --remove-port=80/tcp --permanent`
    
- **Allow Outbound Traffic on Specific Port** (e.g., allow HTTPS outbound traffic on port 443):
    
    bash
    
    Copy
    
    `sudo firewall-cmd --zone=public --add-port=443/tcp --permanent`
    

### 3. **Allow Outbound Traffic to a Specific IP Address**

To allow outbound traffic from your system to a specific IP address, you can add a rule for that:

- **Allow Outbound Traffic to a Specific IP** (e.g., `192.168.1.100`):
    
    bash
    
    Copy
    
    `sudo firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" destination address="192.168.1.100" accept' --permanent`
    

### 4. **Allow Outbound Traffic for Specific Services**

You can allow specific services to communicate outbound. For example, to allow HTTP (port 80) and HTTPS (port 443) outbound traffic:

bash

Copy

`sudo firewall-cmd --zone=public --add-service=http --permanent sudo firewall-cmd --zone=public --add-service=https --permanent`

### 5. **Limiting Outbound Traffic by Protocol**

To allow traffic for a particular protocol like **TCP** or **UDP**:

- **Allow Outbound TCP Traffic** (e.g., to port 443):
    
    bash
    
    Copy
    
    `sudo firewall-cmd --zone=public --add-port=443/tcp --permanent`
    
- **Allow Outbound UDP Traffic** (e.g., to port 53 for DNS queries):
    
    bash
    
    Copy
    
    `sudo firewall-cmd --zone=public --add-port=53/udp --permanent`
    

### 6. **Make the Changes Permanent**

To ensure that changes persist after a reboot, you should always use the `--permanent` flag.

- **Reload firewalld to Apply Permanent Changes**: After making permanent changes, reload `firewalld` to apply them:
    
    bash
    
    Copy
    
    `sudo firewall-cmd --reload`
    

### 7. **Verify the Configuration**

To verify your outbound traffic rules or see the current configuration for outbound traffic:

- **List All Outbound Rules**:
    
    bash
    
    Copy
    
    `sudo firewall-cmd --zone=public --list-all`
    

This will show you which services, ports, and other configurations are allowed or denied in the `public` zone.

### 8. **Blocking All Outbound Traffic (if needed)**

To block all outbound traffic except what is explicitly allowed (which is not common but can be done for extra security):

- **Block All Outbound Traffic**: First, deny all outbound traffic in the default zone:
    
    bash
    
    Copy
    
    `sudo firewall-cmd --zone=public --set-target=DROP --permanent`
    
- **Allow Specific Outbound Traffic** (for example, HTTP and HTTPS):
    
    bash
    
    Copy
    
    `sudo firewall-cmd --zone=public --add-service=http --permanent sudo firewall-cmd --zone=public --add-service=https --permanent`
    

### 9. **Removing Outbound Rules**

If you want to remove a previously added outbound rule, use the `--remove` flag:

- **Remove a Specific Port for Outbound Traffic**:
    
    bash
    
    Copy
    
    `sudo firewall-cmd --zone=public --remove-port=443/tcp --permanent`
    

### 10. **Allow Outbound Traffic for Specific Network Interface**

If you want to restrict outbound traffic to a specific network interface (e.g., `eth0`), you can use:

- **Allow Outbound Traffic Only on a Specific Interface**:
    
    bash
    
    Copy
    
    `sudo firewall-cmd --zone=public --add-interface=eth0 --permanent`
    



### Conclusion

In `firewalld`, the `--direct` option allows you to add, remove, or list rules directly in the underlying iptables chains, bypassing the higher-level firewalld zones and rules. This can be useful when you need to apply specific, low-level firewall rules that aren’t supported by the regular firewalld zones.

To add an outbound rule using the `--direct` option, you would typically target the `OUTPUT` chain, which is responsible for handling outbound traffic. Here’s an example of how to use a direct outbound command in firewalld:

### Example: Add an outbound rule to allow traffic on port 80 (HTTP)

bash

Copy

`firewall-cmd --direct --add-rule ipv4 filter OUTPUT 0 -p tcp --dport 80 -j ACCEPT`

### Breakdown of the command:

- `firewall-cmd`: The command-line interface for managing firewalld.
- `--direct`: Tells firewalld to add the rule directly to the iptables.
- `--add-rule`: Adds a rule.
- `ipv4`: Specifies that the rule is for IPv4 traffic.
- `filter`: Specifies the filter table (the default for iptables).
- `OUTPUT`: The chain to which the rule will be added (in this case, for outbound traffic).
- `0`: The priority (rules are applied in order; lower values take precedence).
- `-p tcp`: The protocol to match (TCP in this case).
- `--dport 80`: The destination port to match (port 80 for HTTP).
- `-j ACCEPT`: The action to take, which is to allow the traffic.

### Example: Remove a direct outbound rule

If you want to remove the rule that you added above, you can use the `--remove-rule` option:

bash

Copy

`firewall-cmd --direct --remove-rule ipv4 filter OUTPUT 0 -p tcp --dport 80 -j ACCEPT`

### Important Considerations:

- Direct rules bypass the firewalld's zones and may not be managed by firewalld itself once set.
- Direct rules are not persistent across reboots unless you explicitly save them.

If you want to make the rule persistent, you should use `firewall-cmd` commands without `--direct` to configure the zones and services directly within the firewalld configuration.