give permission to individual user of that data binary etc 
permissions like read write execute etc 
in other special permissions we give permissions to all  group all other user etc
but in acl we give permission of data to individual use
to use acl ensure your filesystem your file system supports them eg - ext4 ext3 xfs 
command for file system 
df -h for file systems 
blkid for type of file system 
 [rpm -qa | grep acl]
 for detail of acl
 if not install so command for installation 
 [yum install acl]
# get -
getfacl --help
getfacl then file directory name-
se us file pe kisko kisko permissions hai wo aa jayegi
# to set acl on data=
acl file ki permission panel ke ankhiri me show hota hai
[+] symbol
# setfacl-
setfacl --help
setfacl -m u:username:permissions filename
[-m] for modify 
if done toh wo file ki permissions ke end me {+} aa jayega
setfacl -m g:grouoname:permissions filename
# to remove acl-
setfacl -x u:username:permissions filename
setfacl -x g:groupname:permissions filename
#kisi bhi data se pure acl permissions hi hatani ho toh
[-b] 
setfacl -b  filename
 ab directory me acl lagaya aur ye karna hai ab jo bhi nai directory bane uspe bhi whi permissions lage jo parent folder pe hai toh
 setfacl -d -m u:username:permissions foldername
 setfacl -d -m g:groupname:permissions foldername
 {-R} se directory ke andaar directory hai usme bhi same permissions lagegi jo parent folder pe hai
 but ab usme naya folder banainge usme nhi lagegi
 Access Control List (ACL): 

    An Access Control List (ACL) in Linux is a mechanism that allows for more granular permissions on f es and directories than the
    traditional user-group-other permission model. ACLs enable you to specify permissions for individual users or groups beyond the file's
    owner, group, and others.

      Key Concepts of ACLs:

      Standard Permissions: Linux files and directories have three standard permission sets: user (owner), group, and others.

      Extended Permissions: ACLS allow you to assign specific read, write, and execute permissions to additional users or groups.

Managing ALs: To use ACLs, ensure your filesystem supports them (e.g. ext3,ext4, XFS).

Enabling ACLs:
    Mount the filesystem with the act option
    # mount To make this
/dev/sdX -o act /dev/sdX /mountpoint
  change permanent, add it to /etc/fstab:
/mountpoint ext4 defaults, acl 2


CHECK ACL-

  rpm -qa I grep acl
  rpm -qi act
  yum install acl
  rpm -ql acl

GETFACL-

    getfacl --help
    getfacl /data2
    getfacl /etc/hostname /etc/os- release

    - -help
    /data2
    /etc/passwd
    /etc/hostname
    /etc/hostname /etc/os- release

    getfacl -R data2

    getfacl -R folder-name


SET ACL-

To set ACL for a user-

      # setfacl --help

      # set fact -m u: username: permissions filename

      # set fact -m u: armour:rwx /data2
      
      # setfacl -m u:armour:rw /etc/os-release

      # set fact -m g:IT:rwx /data

REMOVE ACL-

      # setfacl -x u:armour /data2

      # setfacl -x g:IT /data2

REMOVE ALL ACL-

      # setfacl -b /data2

      # setfacl -b folder-name

      # setfacl -R -d folder-name

Default ACLs:

      Set default ACLs for directories (applies to new files and subdirectories):
        
        # setfact -d -m u:username:permissions dirname
        
        # set fact -d I-m u:armour:rwx /data2

      use the setfacl for the normal file to do all the changes
      setfacl -l  se permission us folder pe dikh jayegi