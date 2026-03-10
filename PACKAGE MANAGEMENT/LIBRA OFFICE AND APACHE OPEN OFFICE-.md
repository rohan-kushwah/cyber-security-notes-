# libraoffice-
	 practical in GUI
     libra office = ms officee of windows  
     wget  practical in GUI
     libra office = ms officee of windows  
    wget  practical in GUI
     libra office = ms officee of windows  
    wget  practical in GUI
     libra office = ms officee of windows  \
         wget -https://download.documentfoundation.org/libreoffice/stable/25.2.0/rpm/x86_64/LibreOffice_25.2.0_Linux_x86-64_rpm_helppack_en-US.tar.gz.torrent
     rpm -ql then package name se jo files aai hai wo dekh lo
    sari fils rpms me milengi
     ab sari ki sari files ko install karna hai aur sari .rpm me hai toh
    rpm -ivh *.rpm
     then libra office install
     ab graphic me libra office dikhega
    then unistall =
     ab ek ek hataoge toh dependencies ki dikkat aayegi
    isliye libobasic ko rpm -qa | grep libobasis
     aur libroffice then tr karke " \n " " " 
    rpm -qa | grep -E "libreoffice|libobasis" | tr "\n" " " then ab puri line by line packages milega
    then rpm -evh then pure package
# apache open office-
apache server install karna hai wget se
wget https://sourceforge.net/projects/openofficeorg.mirror/files/4.1.15/binaries/en-US/Apache_OpenOffice_4.1.15_Linux_x86-64_install-rpm_en-US.tar.gz/download
tar -xvf then file name
then ab en-us ke nam ka folder banega
RPMS KE andar files milegi
ab inka packaage .rpm me hai toh ab
rpm -ivh * .rpm
 thn ab menu me nhi dikhega gui me
 ab lana ho toh desktop integration me noarch file ko install karo
 rpm -ivh openoffice4.1.15-redhat-menus wali file install kar lo
 ab wo gui me dikhega
 display se hatana ho toh rm -rf en-us delete kar do 
 ab whi process se hata do 
 rpm -qa | grep openoffice | tr "\n" " "
 then rpm -evh then pure package name 
# kisi package se ek aadi file delete ho jaye toh-
rpm  -qlp package name
rpm2cpio then package name
rpm2cpio packagename  | cpio -idmv
rp to cpio phir us cpio ko dump karwa liya
ab wo package jha jha file place karna chahta tha wo files un folder ke sath le ayega
# ab kisi iso file ko mnt karwana hai toh-
wget se iso download kar lo
file then package name deke toh uska filetype aayega
then mount - mount -t then filetype thn file name /location jha mount karna hpo
then ab umount kar do jha mnt kiya hai
