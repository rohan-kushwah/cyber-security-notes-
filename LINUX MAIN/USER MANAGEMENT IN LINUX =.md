linux = [file based operating  system]
what is user  account ?
[user  account represents  user] employee] in company every daily user of the computer have their own user account
user account fact -
every user have unique[UID] user id
user info such as uid username , home directory,shell, stored in /etc/passwd file
ab mujhe koi user acc create karna ho toh /etc/passwd file me entry karni hogi toh wo create ho jayega
three types of user used in linux =
#root user - used as administrator user , perform administrator task jab linux install karte hai tab banta hai , delete nhi kar sakte hai , disable kar sakte hai means- password disable kar sakte hai 
har chij ki permission hoti hai 
root user [UID] = 0 always
#regular user - normal user jab linux install karte hai jab ek banta hai uske bad me bhut sare ban jate hai  iske pass kuch hi chijo ki permission hoti hai routine work ke liye use karte hai isko delete aur disabled bhi kar sakte hai
user id range
min -1000
max - 60000
range modify kar skte hai 
#service or system account - jab hum kuch install karte hai packages ko tab service account create hote hai us package ko run karne ke hi liye service acccount use hota hai domestic use ke liye use nhi kar sakte 
user id range
min- 1
max -999
user ki security setting - /etc/shadow
# home directory -
Root = /root
normal user = /home/user name
# what is group -
group is a collection of user that make easy for
administrator to perform administrator task , ek group pe policies lagao sab user pe lagane ki jarurat nhi
group facts - jab user banta hai toh uske pass do group hote hai primary and secondry 
primary group hoga hi 
only one primary group has asssign to user
primary group ka apna [GID] hota hai 
har group ka unique [GID ] Hota hai 
group info such as gid groupname and member of group stored in /etc/group
security setting group ki /etc/gshadow