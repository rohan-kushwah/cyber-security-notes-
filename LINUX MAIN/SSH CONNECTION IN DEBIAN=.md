normal user se login'
terminal me su root
sudo apt update
sudo apt install openssh-server
nano /etc/ssh/sshd_config
![[WhatsApp Image 2025-01-08 at 21.14.01_43248f3e.jpg]]
dusri line me entry karni hai upar wali line ki
then ab connection ban jayega
physical login of root in debian
2)nd thing to enable root physical login

/etc/pam.d/gdm-password
3rd line me# ! WALI
SAVE AND EXIT
then google ai open jo bolo screen share karo sab batayega
then scp protocol secure copy protocol -
scp fromuser<ip>:"pathfrom"  userto@<ip>:"pathtodestination" 
scp fromuser<ip>:"pathfrom"  userto@<ip>:"pathtodestination"
> scp Downloads/LibreOffice_25.2.0_Linux_x86-64_rpm.tar.gz root@192.168.173.110:/root/ jha se file bhejna ho wh se chalao
> jis pc se bhej e uska ip nhi dena hai