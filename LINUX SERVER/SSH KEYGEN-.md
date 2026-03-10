### Ssh-Keygen-Usage-and-SSH-Authentication-Setup


This document provides a comprehensive guide to generating SSH keys, configuring public key authentication, setting appropriate permissions, and connecting to remote servers using private and DSA key generation and usage.

---

### **Generate A Basic SSH Key Pair**

This command generates a default RSA key pair and stores it in `~/.ssh/id_rsa` and `~/.ssh/id_rsa.pub`.


`ssh-keygen`

---

### **Generate An RSA Key Without A Passphrase**

This creates an RSA key with no passphrase for passwordless login.


`ssh-keygen -t rsa -q -N ""`

---

### **Add The Public Key To Authorized Keys**

Copies the public key to the list of authorized keys on the local system to allow SSH login.


`cp /root/.ssh/id_rsa.pub /root/.ssh/authorized_keys`

### **SSH Into A Server Using A Custom Key**

Specify the key file while connecting to a remote server:


`ssh -i id_rsa_centos root@192.168.1.38`

---

### **Generate RSA Keys With Different Bit Sizes And Passphrases**

#### **2048-Bit RSA Key With A Passphrase**


`ssh-keygen -b 2048 -t rsa -f /tmp/id_rsa -q -N "armour123"`

#### **2048-Bit RSA Key Without A Passphrase**


`ssh-keygen -b 2048 -t rsa -f /tmp/id_rsa1 -q -N ""`

#### **Default Key Location With A Passphrase**


`ssh-keygen -b 2048 -t rsa -f /root/.ssh/id_rsa -q -N "armour123"`

#### **Simple RSA Key Generation With Filename And Passphrase**

(Partially cut off in the image, but seems similar to the above)


`ssh-keygen -t rsa -f /tmp/id_rsa3 -N "armour123"`


### **Check The Key File Type**

Check the type of the generated private and public key files:


`file id_rsa file id_rsa.pub`

---

### **Move The Private Key To The SSH Directory**

Use `-v` for verbose output during the copy:


`cp -v id_rsa .ssh/`

---

### **SSH Without A Custom Key**

Connect to a server using the default identity file:


`ssh root@192.168.1.25`

---

### **SSH Using Specific Key And Key Type Option**

Use a custom identity file and specify acceptable public key types:


`ssh -o PubkeyAcceptedKeyTypes=ssh-rsa root@192.168.1.33 -i id_rsa3`


### **Generate A DSA Key**

#### **1024-Bit DSA Key Without A Passphrase**


`ssh-keygen -b 1024 -t dsa -f /tmp/id_dsa -q -N ""`

#### **DSA Key With A Passphrase**


`ssh-keygen -t dsa -f /tmp/id_dsa -q -N "@rmour123"`

---

### **Add DSA Public Key To Authorized Keys**


`cp id_dsa.pub /root/.ssh/authorized_keys`

---

### **Set Proper Permissions On Authorized Keys**

d

`chmod 644 /root/.ssh/authorized_keys`

![[Pasted image 20250424163653.png]]


## **Managing IP Allow And Deny In SSH**

This guide explains how to manage SSH access control by editing the SSH daemon configuration file to explicitly allow or deny access for specific users or IP addresses.

---

### **Edit The SSH Configuration File**

Use a text editor to open the SSH configuration file:


`vim /etc/ssh/sshd_config`

Make the necessary changes using the `AllowUsers` or `DenyUsers` directives.

---

### **Restart The SSH Service**

After editing the configuration, restart the SSH daemon for the changes to take effect:


`systemctl restart sshd.service`

![[Pasted image 20250424164614.png]]


then systemctl restart sshd
![[Pasted image 20250424165118.png]]
