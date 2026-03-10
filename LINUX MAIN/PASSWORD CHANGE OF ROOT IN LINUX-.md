password bhul jate hai root ka bina kisi setuid lgai kisi binary pe ya bina acl add kare ya bina wheel group ma member banai 
jab hum different ways use karte password change karne ke lie- 
Reset root user password in linux

Method 1: Using a Live Boot
ROOT PASSWD CRACK 


install ubuntu 20.04.06 https://releases.ubuntu.com/20.04/

try ubuntu in the menu 

files > other locations 

see if it is mounted or not 

if mounted 

	then 

	sudo su - 

	mount /dev/ount-name /mnt 

	cd /mnt/etc 

	apt install vim 

	vim /mnt/etc/shadow 

	change the root passwd by openssl passwd -1 (give the desired passwd)

	save and exit 

	check through fg if any current job is running or not 

	home location ~ 

	touch /mnt/.autorelabel

	umount /mnt 

	exit  

	reboot 

	change the boot order

	login to centOS

Method 2: Using Recovery Mode

1. Reboot the system: Reboot the system.
2. Select recovery mode: Select the recovery mode option from the GRUB boot menu.
3. Select the root option: Select the "root" option from the recovery menu.
4. Mount the filesystem: Mount the filesystem in read-write mode.

mount -o remount,rw /

1. Reset the root password: Reset the root password using the passwd command.

passwd root

1. Reboot: Reboot the system and login with the new root password.

Method 3: Using GRUB Boot Loader in centos-
1. f2  ya f12 sese grub looder pe
     Reboot the system: Reboot the system.
2. Select the GRUB boot menu: Select the GRUB boot menu.
3. Press 'e' to edit: Press 'e' to edit the boot options.
4. Find the 'ro' line: Find the line that starts with "ro" and replace it with "rw init=/sysroot/sbin/sh".
5. Press 'Ctrl+X' to boot: Press 'Ctrl+X' to boot with the modified options.
6. Chroot into the system: Chroot into the system.

chroot /sysroot

1. Reset the root password: Reset the root password using the passwd command.

passwd root

1. Check SELinux status: Check the SELinux status.

sestatus

If SELinux is enabled, create a file .autorelabel and reboot.

touch /.autorelabel
reboot

1. Reboot: Reboot the system and login with the new root password.

Method 4: Using Debian Recovery Mode - for kali and debian like-

1. Reboot the system: Reboot the system.
2. f2 se andar jayenge 
3. Select the GRUB boot menu: Select the GRUB boot menu.
4. Press 'e' to edit: Press 'e' to edit the boot options.
5. Find the 'ro quiet' line: Find the line that starts with "ro splash" and replace it with "rw singel init=/sbin/sh".
6. Press 'Ctrl+X' to boot: Press 'Ctrl+X' to boot with the modified options.
7. Reset the root password: Reset the root password using the passwd command.

passwd root

1. Reboot: Reboot the system and login with the new root password.