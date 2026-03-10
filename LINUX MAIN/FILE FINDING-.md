to install give command yum install tree
tree command direct give tree stucture of all files
syntax ' tree then jiski files dekhni ho' 
ex - tree /home/
home ki sari le aayega
ex - tree /home/d1/
toh d1 ke andar ki sari files la dega
we have -a -for hidden files
-v foe vervus
-r for reverse
-t for time
drawback sabhi files nhi la ra
## find command-
yum install findutils
syntax  find [then jha pe content hai wo file] then -name [then jo file chaiye] 
ex - find / -name  .conf toh .conf wali files aa jayegi
we have multiple switch in this command
-type f ya d 
f-file 
d - directory
-readable jo sirf pad sakte ho
-executable[x] jo run kar sakte ho
-writable jisme changes kar sakte ho
user jo nam denge wo user jisko permission chaiye 
particular file dhudna hai jiska owner   rohan ho
find / -name .conf user rohan
 ex - find / -iname  root
 i for both capital and small
 yaha wildcard use ke pehle slash ka use karna hoga
 ex find / -name pass/* 
 aisi line lagaiga jo pass se start hogi'
 tenno switch ko sath me bhi use kar skte hai readable writable executable
 aisi file find karni hai jisk permission ki id 1000 ho
 find / -perm=1000 -o=rwx 
 aur jo readable writeable aue executable ho
 find /etc/passwd -exec date [\];  back slash]
 id chala ke dekh lo
 # locate command-
 yum install mlocate
 update db data base index banaiga files ka 
 locate -S data base file ka data bataiga
 syntax locate  [yaha jha se dhundni hai  wo file bhi de sakte hai filename jo dhundna ho ]
 data base se dhund lega
 -i for small and capital
 then extensions dekh lo locate karke ke
 # which and whereis command-
 binaries or command jo run karte hai wo bin aur sbin me rakhi hoti hai
 [echo $Path se binary ki location or path pata lag jayeega ]
 which command in locations pe jake binaries ko pick karta hai
 ex - which ls
 which vim 
 # compare command-
 do file ko compare karna hai toh -
 comm file.1txt file.2txt
 # difference command-
 diff -c file1.txt file2.txt or -y se bhi kar sakte hai
 vimdiff 1.txt 2.txt best to compare
 
 

