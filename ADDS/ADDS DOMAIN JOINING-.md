firstly go to clients give the ip of adds server in thier dns
then from client side ping server and nslookup of fqdn
then domain join go to pc properties then domain or workgroup
fqdn set  ho toh hata dena
then workgroup ko uncheck karke domain 
domain do jo server ko di hai
then you have prompt for id password
admin ka do  server hand ke admin ka do dusre user jko doge toh wo jyada user ko domain join nhi karwa sakta
then ping and nslookup
other user ke pass domain join karne ki power to hoti hai par wo domain unjoin nhi kar saakta
then apke pass local user aur domain join user bhi milega
local user se login karke ek bar firewall check kar le domain join kaarte hai us smay firewall on ho jata hai
then dns record aa jayega'
then adds user me dekh le
jo  user login hai dikh jayenge 
manage me jake
then apke pass user hai uske full access hai apke pass
user konse login honge konse nhi honge admin ko diasable sab kar sakte hai
mene ab admin or local user ko disabled kar diya ab us pc pe sirf domain wala pc join hoga
then server pe jake user pe jake do user bana diye 
then display name bhi dedo
server ke users client pe login kar sakte  hai
profile client pe hi banegi local addmin ja saakta hai
then admin enable kar ke login karwa lo
login karne ke liye local pc name chiaye pata ho toh thik hai'\
nhi toh .\then user name
.\ se niche pc name aa jayega wo dena hai then\username
then  domain administer login karna hai toh wo local pc pe le jayegz
aur local pc ka administrator disable hota hai
toh administrator@hardia.local
then ab wo domain admin hai wo kisi bhi user ki profile me ja sakta hai
another way middle domain name de do \ usernsme
then domain user se login karra
then pc ki properties me jake unjoin nhi karwa sakte
local admin karwa sakta hai unjoin
workgroup enable
then give id password 
the no control of this user to server 
enntry ayegi but control gaya
domain ka admin bhi unjoin karwa sakta hai
password nhi manega
adds remove karo jab server ka fqdn hata dens
# adds - active directory domain services
centralized way of managing group users etc
work on kebros and ntlm protocols
### 1. **Kebros (Kerberos) Protocol:**

**Kerberos** is a network authentication protocol designed to provide strong authentication using secret-key cryptography. It is widely used in environments such as Microsoft Active Directory and other enterprise-level systems to ensure secure communication between systems and user

### 2. **NTLM (NT LAN Manager) Protocol:**

**NTLM** is an older authentication protocol used by Microsoft Windows for authentication. While it's still in use, it is considered less secure than newer protocols like Kerberos, and it is recommended to use Kerberos in modern networks if possible. NTLM is mainly used for backward compatibility and in scenarios where Kerberos is not support

### Summary of Ports:

- **Kerberos:**
    - **Port 88** (UDP/TCP)
- **NTLM:**
    - **Port 139** (TCP)
    - **Port 445** (TCP 
    # hierarchical structure of AD-
    - 1. ### 1. **Forest**

- The **forest** is the topmost container in Active Directory. It represents the highest level of the AD structure and encompasses all domain trees. A forest is typically associated with a single organization, but it can contain multiple domains, each with its own set of policies and configurations.  
- 2. ### 2. **Domain Tree**

- A **domain tree** is a collection of domains within the same forest that share a contiguous namespace. Domains within a tree are organized hierarchically, with parent-child relationships between them. Each domain in a tree shares a portion of the overall namespace.
- **Key Features:**
    - Domains in a tree can have subdomains, forming a hierarchical tree structure.
    - ### 3. **Organizational Unit (OU)**

- An **Organizational Unit (OU)** is a container within a domain that allows administrators to group and manage objects. It can be used to delegate administrative rights, assign Group Policy settings, and organize objects for easier management.
- **Key Features:**
    - OUs are used to organize users, groups, and computers within a domain.
    - adds give single point access
    - single sign on bhi deta
    - group policy bhi deta


 1. ### 4. **Domain**

- A **domain** is a logical group of network objects, such as users, computers, and devices, that share the same AD database. It is a core unit in the AD hierarchy.
- **Key Features:**
    - Each domain has its own security policies, such as user authentication and authorization.
    - A domain has a unique DNS name, such as `example.com`.
    - Domains have Domain Controllers (DCs) that store a copy of the AD database.


    - #physical componennts of adds=
    - domain controller [dc] 
    - server that store add data and provide authentication and authorization services
    - every  dcc holds a writeable copy of ad data base 
    - kisi server me ek se jyada dc ho sakte hai
    - #global catalog - database hota hai contain karta hai searchable objects ka index aur usse forest me rakhta hai
    - #read only domain controller- 
    - a domain controller jiske pass  read only copy ho add databse ki
    - #site - represent physical structure of network
    - #replication - sycronize database of add
    - #data store- adds ka data ntds.dit me save hota hai
    - #end 