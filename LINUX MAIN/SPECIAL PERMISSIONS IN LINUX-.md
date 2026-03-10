yadi koi binary [command] root se run hogi toh ab dusre user bhi use run kar pai  toh kya kare use root bana de isse acha toh wo binary ko special permission de do ki wo jis bhi user se run hogi but wo root ki power se run hogi
called setuid - special permission for linux
chmod u+s  then binary name
toh ab jha bhi binary run hogi wo run as root hogi 
like fdisk command ko which karke location nikal do
the ab local user se login karwake binary run karke dekho toh wo nhi hogi
but usko special permission dene ke bad wo run kar payega
vim ko special permission de toh wo jha bhi run hoga run as root hoga toh wo dusre user jha uske pass permission  nhi thi particular file  ko dekhne ki ab usko edit kar sakte hai
ls ko deke dekh lo ab jaise kisi bhi file ko dekhne ki permission
user ek dusre ki home directory nhi ja sakte but ls se dekh toh sakte hai na
thi ls ko special permission de do
number se permissionn de do=
chmod 4755 file
4 for special permission
7 for rwx
5 for rx
5 for rx
then opt  me jao
yum install gcc  called complier
then vim rootshell.c - call shell of linux
isme google se script dalo then run karke dekho./ ke sath
#include <stdio.h>
int main(void)
{
setuid(0);
setgid(0);
seteuid(0);
seteguid(0);
execvp("/bin/sh",NULL,NULL);
}
dono user me
then compile c to binary
gcc rootshell .c -o rootshell
then id command
then dusre user me shell run karke dekho
then id toh us user ki gid uid aayeegi
ab use binary ko special permission de denge toh wo run as root hogi jaha bhi chalao
then  vim karke script ke andar zero ko jagah 1000 toh wo rohan se chalegi
then compile gcc rootshell.c  -o rootshell
then ab uska user rooot se rohan kar do toh wo root se bhi run hoga toh bhi uid user ki dega
ab ushell kar owner hi usko special permission de aur hata sakta hai
then permission on passwd , shadow, gshadow , group
group and passwd have given read permission to other user 
gshadow and shadow have nlo permission given to any user 
but  only run on root
so jab bhi hum isme direct password dalenge toh wo sabko dikhega
shadow and gshadow jab bhi user ke sath changes hoga toh wo bhi update hogi
passwd command pehel se setuid se encrypted aati hai 
passwd by default root se chalegi kyunki use update karna rehta hai data ko in shadow and gshadow file special permission hatai toh koi change nhi karega in files me
then root ka alias bana do with same uid gid etc and password
but wo login as a root hi hoga'
![[WhatsApp Image 2025-01-10 at 18.32.53_096e6a1a.jpg]]
read