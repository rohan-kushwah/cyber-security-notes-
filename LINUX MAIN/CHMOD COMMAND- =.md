# permission allocating-
#symbolic method - 
r -read
w-write'
x-execute
synatx-
user class = operation=permission
user class -
1. u -user owner
2. g- group
3. o-others
4. a -all other user groups etc 
operation (=,-,+)
[+]adding permissions
[-] removing of permissions
[=] do permission add karna ho toh 
permissions - r , w, x
example - chmod u+r file.txt
chmod o-w file1.txt
chmod g=rw file2.txt
#numeric method  
read = 4
write=2
execute =1
user class 
1. owner [u] ke liye 
2. group ke liye
3. others ke liye
  then jaise r+w+x ki permissions kisi user ko deni hai toh 
chmod  644 file.txt
ab owner ke pass r=w ki permission hai
group ke pass read ki
aur other user ke pass bhi read ki
11th permission
[.] hai toh no acl
[+] hai toh acl hai
{=} ka use karoge toh jo permissions de rhe ho whi lagegi baki jo hogi hat jayegi