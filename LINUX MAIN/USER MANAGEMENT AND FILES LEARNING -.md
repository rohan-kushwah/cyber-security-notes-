system account me wo root ko bhi ginta hai isliye service system has minor difference
cat /etc/login.defs 
isme uid and gid ka proper configuration mil jayega
grep -v '^#' /etc/login.defs | grep -v "^$"
then file summary-
user ka mail pehli line me ek user dusre user ke liye mail pass kare toh wo location se kar sakta hai 
A umask value is ==a three-digit octal number that specifies which file permissions are not to be given to newly created files and directories==: 

- **First digit**: Permissions for the user
- **Second digit**: Permissions for the group
- **Third digit**: Permissions for others (users who are not the owner or in the group)

The umask value is used in conjunction with the access mode 666 for files or 777 for directories to determine the access mode for new files and directories. The umask value is XORed with the access mode, which results in the opposite of what is represented by the mask. 

Here are some examples of umask values and their effect: 

- **0000**: Permits created files to have read, write, and execute access
- **022**: Results in permissions of 644
- **027**: More restrictive than 022
- **077**: Denies all permissions for the group and others
then home directory pe permission lagu ho ro wo wha se pata lag jayegi
then age of password wo 1970 se count hoga
minimum age matlab abhi password change kiya toh aur kar sakte hai
warning dega 7 din pehle ki password expire hora
then aata hai 
encrypt method -password set karenge ya use karenge user ka wo safe or encrypted hai by sha512
Password encryption is ==a process that scrambles a password into an unreadable and unusable form to protect it from hackers==.
then kitte rounds me encrypt karga
# imp - 
do user and group ki same uid andd gid ho sakti hai wo login alg alg ho skate hai but wo ek hi group ya user count honge'
two types of group -
system group -jo pehle se bane hue aate hai
user group - jo hum banate hai
