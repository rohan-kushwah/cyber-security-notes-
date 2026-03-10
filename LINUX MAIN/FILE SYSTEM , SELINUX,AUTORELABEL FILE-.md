#Linux supports various file system types, such as **ext4, XFS, Btrfs, and ZFS**, each offering different features like journaling, scalability, and snapshot capabilities. The most commonly used file system on Linux is ext4, known for its balance between performance and reliability.24 Oct 2024
#### A Realistic View of Files

Files have names, metadata (such as permissions and an owner), and data. All of the elements associated with files are not stored in one place on a disk. The filename is stored as part of the directory, the metadata is stored in an inode, and the file data is stored across multiple logical blocks on the disk. The hardware driver further abstracts the physical disk locations of all of the data associated with that file. For the sake of usability, the operating systems will hide these details when you interact with files through a terminal or some type of graphical user interface (GUI).

# file Inodes

Every file is assigned one inode when it is created. The purpose of an inode is to maintain information about the file (e.g., permissions) as well as which blocks on the file system make up the data portion of the file (see Figure 2.1).

![](https://ars.els-cdn.com/content/image/3-s2.0-B9781597490184500095-f02-01-9781597490184.jpg)
# DISCRETIONARY ACCESS CONTROL
DAC, also known as file permissions, is the access control in Unix and [Linux systems](https://www.sciencedirect.com/topics/computer-science/linux-system "Learn more about Linux systems from ScienceDirect's AI-generated Topic Pages"). Whenever you have seen the syntax drwxr-xs-x, it is the ugo abbreviation for owner, group, and other permissions in the directory listing. Ugo is the abbreviation for user access, group access, and other system user's access, respectively. These file permissions are set to allow or deny access to members of their own group, or any other groups. Modification of file, directory, and devices are achieved using the chmod command. Tables 77.1 and 77.2 illustrate the syntax to assign or remove permissions. Permissions can be assigned using the character format:

 # Mandatory access control (MAC)
  is ==a security system that restricts access to resources based on a user's clearance level and the sensitivity of the resource==. MAC is enforced by the operating system or application, and cannot be overridden by users. 

How it works

- **User clearance**
    
    Users are assigned a clearance level based on their authorization to access information 
    

- **Resource sensitivity**
    
    Resources are assigned a sensitivity level based on the confidentiality of the information they contain 
    

- **Access control lists**
    
    Access control lists (ACLs) specify user permissions and check clearance levels 
    

- **Security labels**
    
    Security labels tag data and users with classification levels 
    

- **Reference monitors**
    
    Reference monitors evaluate access requests against ACLs and security labels
    ![[Pasted image 20250123181128.png]]
    Linux provides various access control mechanisms to manage file and system access for users, groups, and processes. Here are the key ones:

1. Traditional File Permissions

Owner, Group, and Others: Each file or directory in Linux has three access levels (owner, group, others) and three types of permissions (read, write, execute).

Permission Representation:
Numeric: chmod 755 filename
Symbolic: chmod u+rwx,g+rx,o+rx filename
Command: ls -l to view permissions.

---

2. Access Control Lists (ACLs)

ACLs provide finer-grained control than traditional file permissions, allowing you to set permissions for specific users or groups beyond the default owner, group, and others.

Commands:

getfacl: View ACLs
setfacl: Set ACLs.
Example: setfacl -m u:username:rwx file.txt

---

3. Special File Permissions

Setuid: Allows a program to run with the permissions of its owner.

Example: chmod u+s file
Setgid: Files inherit the group ID of their parent directory
Example: chmod g+s directory
Sticky Bit: Restricts file deletion in shared directories to the owner.
Example: chmod +t /tmp

---

4. Role-Based Access Control (RBAC)

In some Linux distributions (e.g., Red Hat), RBAC is used to assign roles to users with specific privileges.

Example: Assign roles via the semanage command (used in SELinux).

---

5. Mandatory Access Control (MAC)

Enforces policies defined by the system, rather than by individual users.

Examples:

SELinux (Security-Enhanced Linux): Defines policies to control what processes can access.
Tools: getenforce, setenforce, semanage.
AppArmor: Profile-based access control for applications.
#Security-Enhanced Linux (SELinux) is ==a mandatory access control (MAC) system that protects Linux operating systems==. It's a part of the Linux kernel that restricts programs from accessing files, data, and other resources without administrator approval.

Tools: aa-status, aa-complain, aa-enforce.

---

6. Capabilities

Linux capabilities divide root privileges into smaller, more specific capabilities that can be independently assigned to processes.

Commands:

getcap: View capabilities.
setcap: Assign capabilities.
Example: setcap cap_net_bind_service=+ep /usr/bin/program

---

7. Namespace Isolation

Used in containerization (e.g., Docker) to isolate processes in separate namespaces, such as PID, user, network, or filesystem namespaces.

Command: unshare to create a new namespace.

---

8. User and Group Management

Control user and group access to resources.

Commands:

useradd, usermod, passwd for users.
groupadd, gpasswd, groupmod for groups.

---

9. Pluggable Authentication Modules (PAM)

Flexible authentication framework.

Files: /etc/pam.d/*

Common modules:
pam_unix.so: Standard password authentication.
pam_tally2.so: Lock accounts after failed login attempts.

---

10. Chroot

Isolates a process and its filesystem by changing its root directory.

Command: chroot /new/root

---

11. File System Quotas

Restrict disk space usage per user or group.

Commands:

quota: Display quotas.
edquota: Edit quotas.
repquota: Report quotas.

---

12. Firewall (Network Access Control)

Controls network traffic to/from the system.
Tools:
iptables, nftables for packet filtering.
firewalld for managing firewall rules.

# The `./autorelabel` = 
file in SELinux ==triggers a relabeling operation when the system boots==. This operation is useful when SELinux policies or file contexts have been modified. 

How it works 

- The `./autorelabel` file is created to trigger the relabeling process.
- The system automatically removes the `./autorelabel` file after the relabeling is complete.
- The relabeling operation ensures that all files and directories have the correct security context.