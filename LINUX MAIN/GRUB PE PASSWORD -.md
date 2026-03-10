grub pe password isliye lagate hai kyunki koi bhi aayega aur root ka password change kar jayega
by process -
netstat -nltup se malum pad jata  hai konse konse ports aur packages install hai
# first way-
boot loader linux me -/boot/grub2/grub.cfg me rehta 
pehel backup bana lo nhi toh lag jayenge
password generate for grub
grub2-mkpasswd-pbkdf2
se password generate kar lega
ab wo password ko hum main file /boot/grub/grub.cfg me daldo ya fir configuration file /etc/grub/grub.d/10_linux me  
configuration file me dal rhe ho toh last me dalo
[cat << EOF]
[set superusers="rohan"]
[password_pbkdf2 rohan space generted password] 
[EOF]
#  second way-
ab nhi ho ra toh main file me place kar do
10_linux complete ho ra uske bad 
[set superusers="rohan"]
[password_pbkdf2 rohan space generted password] 
then robot ab password milega boot loadder pe
boat loader ka password bhul gaye toh battery nikal do
jo line lagai hai unhe hata do password hat jayeega
# third way-
grub ki main file me username aur password ka block hai usme dekh lo username aur passsword wo kaha place kar ra
by command to set grub passsword-
#grub2-setpassword
toh ye password laga dega grub pe
then reboot password change 
ab gurb ka passwd bhul gai root ka bhi toh ab like boot karke jha username aur password change ho ra wo file hata do
/boot/grub2/ me user.cfg wali
chroot /mnt/sysroot milega after liveboot
