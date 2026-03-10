A **Secondary DNS Server** is a backup DNS server that obtains its DNS records from a **Primary DNS Server** through a process called **zone transfer**. It serves as a redundancy mechanism to improve DNS availability, performance, and fault tolerance.

### **Key Features of a Secondary DNS Server:**

1. **Read-Only Copy** – It holds a replicated, read-only copy of the DNS zone from the primary DNS server.
    
2. **Zone Transfers** – It synchronizes data from the primary DNS server using **AXFR (full transfer)** or **IXFR (incremental transfer)**.
    
3. **Load Balancing** – It helps distribute DNS query loads, reducing stress on the primary DNS server.
    
4. **Fault Tolerance** – If the primary DNS server goes down, the secondary DNS can still respond to queries.
    
5. **Automatic Updates** – Changes made on the primary server propagate automatically to the secondary server.
    

### **Primary vs. Secondary DNS Server**

|Feature|Primary DNS Server|Secondary DNS Server|
|---|---|---|
|Data Control|Read-Write (can be edited)|Read-Only (cannot be edited)|
|Zone Transfers|No, it is the authoritative source|Yes, receives zone transfers from Primary|
|Role in Failover|First point of resolution|Backup if Primary fails|
|DNS Query Handling|Handles requests|Assists in load balancing|

### **How to Set Up a Secondary DNS Server in Windows Server 2022**

1. **Install DNS Server Role** on a second Windows Server machine.
    
2. **Configure a Secondary Zone** in DNS Manager.
    
3. **Specify the Primary DNS Server** as the master.
    
4. **Allow Zone Transfers** from the primary server.
    
5. **Test DNS Resolution** to ensure failover works.


### **Master and Slave DNS Server**

This guide covers the configuration of a **Master DNS Server** and a **Slave DNS Server** using **BIND** (Berkeley Internet Name Domain).

### **Server Details**

| **Role**    | **IP Address**  | **Description**      |
| ----------- | --------------- | -------------------- |
| Master DNS  | 192.168.180.139 | Primary DNS server   |
| Slave DNS   | 192.168.180.140 | Secondary DNS server |
| Domain Name | hardia.local    | Example domain       |

## **Master DNS Configuration**

#### **Step 1: Set the Hostname**

Set the hostname for the Master DNS server:


`hostnamectl set-hostname ns1.hardia.local`

#### **Step 2: Install BIND**

Install the BIND package:

`yum install bind -y`


### zone transfer of hardia.local
### **Step 3: Configure the DNS Zones**

Edit the zone configuration file:

`vim /etc/named.rfc1912.zones`

#### **Example configuration:**

```

zone "loopspell" IN {
    type master;
    file "/var/named/loopspell";
    allow-update { none; };
    allow-transfer { 192.168.180.140; };
};

zone "IT.local" IN {
    type master;
    file "named.IT";
    allow-update { none; };
    allow-transfer { 192.168.180.140; };
};

zone "HR.local" IN {
    type master;
    file "named.HR";
    allow-update { none; };
    allow-transfer { 192.168.180.140; };
};
```


### **Step 4: Create the Zone Files**

Create the forward lookup zone files:

#### **Forward Zone for `hardia.local`**

`vim /var/named/forward.armour.local`



**Example content:**
```
$TTL 1D
@   IN  SOA ns1.hardia.local. root.hardia.local. (
        002     ; serial
        1D      ; refresh
        1H      ; retry
        1W      ; expire
        3H      ; minimum
)
@   IN  NS      ns1.hardia.local.
 @   IN  NS      ns2.hardia.local.
ns1 IN  A       192.168.180.139
ns2 IN  A       192.168.180.140
hardia.local. IN A 192.168.180.139
www IN  CNAME   hardia.local.
  ```

ho to ns aur a dono bana do server ka har zone me


### **Step 5: Configure Firewall Rules (Firewalld)**

**Allow** DNS Traffic (TCP and UDP Port 53):
```
firewall**-cmd --permanent --add-port=53/tcp
firewall-cmd --permanent --add-port=53/udp
firewall-cmd --**reload****
```



### **Step 6: Validate Configuration**

Check the configuration files:
```
```named-checkconf
named-checkconf /etc/named.conf
named-checkconf /etc/named.rfc1912.zones
named-checkzone HR.local /var/named/named.HR
named-checkzone IT.local /var/named/named.IT

```

### **Step 7: Start and Enable BIND**

Start and enable the BIND service:


`systemctl restart named.service`
`systemctl enable named.service`

### **Slave DNS Configuration

#### **Step 1: Set the Hostname**

Set the hostname for the Slave DNS server:


`hostnamectl set-hostname ns2.hardia.local`

---

#### **Step 2: Install BIND**

Install the BIND package:


`yum install bind -y`

---

#### **Step 3: Configure /etc/hosts**

Edit the `/etc/hosts` file:


`vim /etc/hosts`


**ADD LAST LINE 
`127.0.0.1   localhost ns2 ns2.hardia.local
`::1         localhost localhost6 ns2 ns2.hardia.local
`192.168.1.40 ns2 ns2.hardia.local


### **Step 4: Configure the Zones on the Slave**

Edit the zone configuration file:


`vim /etc/named.rfc1912.zones`

```
zone "hardia.local" IN {
    type slave;
    masters { 192.168.180.139; };
    file "slaves/forward.hardia.local";
};

zone "IT.local" IN {
    type slave;
    masters { 192.168.180.139; };
    file "slaves/forward.IT.local";
};

zone "HR.local" IN {
    type slave;
    masters { 192.168.180.139; };
    file "slaves/forward.HR.local";
};
```


Allow DNS Traffic (TCP and UDP Port 53):
```
firewall-cmd --permanent --add-port=53/tcp  
firewall-cmd --permanent --add-port=53/udp  
firewall-cmd --reload
```

### **Step 6: Start and Enable BIND**

Start and enable the BIND service:


`systemctl restart named.service  
`systemctl enable named.service`  

---

### **Step 7: Validate Configuration**

Check the configuration files:

`named-checkconf   
`named-checkconf /etc/named.conf  
`named-checkconf /etc/named.rfc1912.zones`


### **Troubleshooting**

#### **Check DNS Service Status**


`systemctl status named.service`

#### **Check Listening Ports**


`netstat -nltup | grep named`



## LAST  ME NAMED.CONF ME LISTENING ME SECONDRY SERVER KA IP AUR QUERY ME PURA NETWORK