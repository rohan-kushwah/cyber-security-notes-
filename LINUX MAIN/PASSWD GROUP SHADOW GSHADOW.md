/etc/passwd  info about - uid gid username , home location ,shell                                                                                              ![[rohan.png]]                                                                                     
x aa ra toh uska password shadow me save hai
comment me uska display name
uid = o root
uid >0 system user
uid > 999 normal user
GID -same as uid 
have seven field                                                                         
# /etc/shadow -contains user settings
![[rohan3 3.png]]yadi $ [#] aa ra toh wo user enable hai aur password set hai
[ * ]  matlab password save nhi hai
[ !] aara matllab user disable hai
last password  change means - kitte din se password change nhi hua count kar ra 1 jan 1970 se
min pass age =  o hai means aaj badla toh aaj wapas badal sakte hai
max password age = kab tak valid hai 1970 se count karega
warning period - expire hone se pehle warning dega
inactive period -matlab kabse active hai
exp date - band kitte din se hai
unused - kitte din se use me nhi aaya 

#encryption of password type -
![[pass.png]]
# /etc/group - gid group name group members 
![[kush.png]] 
 x hai matlab gshadow me password save hai
 gid group id
 then member of group
 # /etc/gshadow - contain group passwords etc 
 ![[har.png]]
 yadi g shadow file delete ho jai toh password group file me jayennge'
 yadi group ke aage space hai toh password encrypted hai
 yadi ! hai toh group disable hai
 then name of group administrator  
 then member of group
 jab user banainge toh charo files sath me update hogi 
 