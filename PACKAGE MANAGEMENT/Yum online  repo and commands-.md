yum repolist se kon si repo hai aur usme kya hai bata dega
 yum.repos.d me jakee vim centos.repo or centos -addons.repo yaha sare repos dikh jayenge
  mirror - jha se packages download karte hai
  ye sab offical repos hai yum repolist se dekh lo
  ab unoffical add karenge
#ISLIYE KAR RE KI PACKAGE DOWNLOAD hum intranet network me bhi kar pai= 
# EPEL  --
  EXTRA PACKAGE FOR ENTERPRISE LINUX 
  ye packages ka database deta
  https://dl.fedoraproject.org/pub/epel/
  yaha se databasee file utha lo
   rpm -ivh [epel-release-latest-9.noarch.rpm](https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm)
  istall kar lo'
  ab yum repolist all me entry bad jayegi
  all lagane se disaable repo  bhi aa jayego
  yum list available | wc -l se kitne package aai dekh lo
# REMI REPO--
EXTRA ONLINE YUM REPOS CONATIN DATABASE OF PACKAGES
 yum [remi-release-9.rpm](https://rpms.remirepo.net/enterprise/remi-release-9.rpm)
 yum list available | wc -l se kitne package aai dekh lo
# RPM FUSION--
yum install https://mirrors.ustc.edu.cn/rpmfusion/free/el/rpmfusion-free-release-9.noarch.rpm
 yum list available | wc -l se kitne package aai dekh lo  
#  EL REPO--
ADD KEY -|# rpm --import [https://www.elrepo.org/RPM-GPG-KEY-elrepo.org](https://www.elrepo.org/RPM-GPG-KEY-elrepo.org "https://www.elrepo.org/RPM-GPG-KEY-elrepo.org")|
|---|
|# rpm --import [https://www.elrepo.org/RPM-GPG-KEY-v2-elrepo.org](https://www.elrepo.org/RPM-GPG-KEY-v2-elrepo.org "https://www.elrepo.org/RPM-GPG-KEY-v2-elrepo.org")|
THEN  ADD DATABASE-# yum install [https://www.elrepo.org/elrepo-release-9.el9.elrepo.noarch.rpm](https://www.elrepo.org/elrepo-release-9.el9.elrepo.noarch.rpm "https://www.elrepo.org/elrepo-release-9.el9.elrepo.noarch.rpm")
# COMMANDS-
yum repolist se enable repos le ke ayega
yum repolist all se sari layegaa
repo enble karni hai toh gpgcheck 0 se 1
yum list - se packages
yum list recent se recent packages
yum check-update- available package ke update dikhayega
yum update= se available package update kar dega
yum upgrade- kernel level tak package update karego
yum downgrade se ek version piche chale jayenge
yum search package name - package dhundegaa
yum info package name - info of package
yum install package name - to install
yum remove package name- to remove
yum provides packagename - konsi repo ke sath package aaya
yum deplist package name - package kispe dependent hai
yum reinstall packagenamee
yum clean all - yum ka cache clear
yum makecache = cache bana dega
cache cd /var/cache/dnf me banega
yum history -- se history




