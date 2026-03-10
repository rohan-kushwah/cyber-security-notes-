YUM, or Yellowdog Updater Modified, is ==a free, open-source tool for managing packages on Linux systems==. It's a command-line utility that uses the RPM package manager to install, update, and remove software packages. 

How it works

- YUM uses software repositories, or repos, to search for packages. Repos are directories that contain collections of software packages. 
- YUM automatically resolves dependencies when installing, updating, or removing packages. 

- YUM works with online repositories, but users can also store their own packages in a local repository. 

Why it's useful YUM simplifies package management, YUM makes it easy to manage dependencies, and YUM automatically updates package
# practical portion- offline yum configuration or offline repo
for use of offline yum in intranet network 
insert iso
mount in location location from sr0
df -h
location location pe jake appstrem ke andar packages rehnge wo package dependencies resolve karte hai
ab unki dependencies puch lo aur unka ek databasse bana lo 
pehle mnt ki andar mount karo then usko jo folder banaya uske andar copy 
then ab umount karwa do mnt  ko
ab folder ke andar jake database bana do
create repo help karega data base banane me 
createrepo -v --database ab location kha banana hai
createrepo packages ke pass jake unki dependencies puchta hai
ab /etc/yum.repos.d me jake packages hata do wo jab online yum chalate hai jab use hota hai
ab wha pe jake ek new repo bana do aur file url me jha database banaya hai uski location de do
yum repollist all se bata dega koi repo aur toh nhi
yum clean all se pehli sari repos ka data clear
yum repolist all se changes repo ke enable kar deta'
yum list available se kitte packages all
 jo repo banate hai hum usme 
 1. repo id
 2. repo name
 3. bash url jha hamne repo rakhi hai
 4. gpgcheck -0 isliye diya ki wo package ki itegrity ko check na karw
ab chalao yum install package name toh ye offline database se answer dega'
yum install vim , nmap , bind, firemox  offline me package hoga wha se answer dega
package ka nam nhi mile toh grep kar lo