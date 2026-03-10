a shell is a command line interpreter allow user to interacts with os by typing and executing command 
shell also works as intermediate between user and kernel
1. sh bourne shell                                                                                                   - focused on executing commands and writing basic scripts                             -supports variable , loops ,conditionals , and redirections 
2. bash bourne shell                                                                                               - same features like [sh]                                                                                    -supports scripting, command history , tab completion                                   
3. korn shell [ksh]                                                                                                    -also called c shell combining feature of sh use in programing enviroment
4. tcsh [tenex c shell]                                                                                              - a enhance version of c shell  , adding editing of command line
5. fish shell                                                                                                              -colouring ke liye use hota hai  , user friendly hota hai aur easy to use hota hai
6. zsh shell                                                                                                              - powerfull and most used shell  advanced fetaures like  shared history , spell checking and themes.                                                                               
7. dash shell                                                                                                             -lightweight and fast shell                                                                                  - used as the default /bin/sh in some systems for scripting  due to its efficency
# shell installation and shell changing 
addons  - alg se add karna chijo ko 
same as plugins 
cat /etc/shells se jo shell install  hai wo wo dekh sakte hai 
echo $SHELL se shell  pata laga sakte hai the shell de do toh wo shell pe phuch jayengee
base version of bash ko update karna hai toh -
yum install bash*
then yum search bash se jo addons install hue hai use dekh sakte hai 
then yum install bash - completion .noarch ko  instal karo toh taab completion ka option chalu ho jayeega
then colour prompt ko install karke dekho
then command chalao 
but wo har bar karna padega toh usko centralized karna hai 
then puri file bhi clour me nhi aa ri toh hum grc ka use karenge 
link se jao [https://github.com/garabik/grc/releases/tag/v1.13]
release pe jake particular file download kar lo
source code tar wali
then downloads me jake wget command ke sath  link bhi de do
wget nhi hai toh yum install wget 
tar file ko untar karo then tar file delete
then mv -v untar file name  /opt/
then opt ke andar folder mil jayega 
install.sh
then install.sh file ko run karna hai toh
./install.sh
corresponding files ko apni locations pe pahucha denge
then run commands aage grc laga ke toh colour ayega
then whi link pe jake jo command support karegi grc toh wo grc.sh me aa jayegi
https://github.com/garabik/grc/releases/tag/v1.13.tar.gz
then ye har bar na karna padde iise acha 
vim /root/.bashrc
user login karta hai toh ye script run hoti hai
 alias me command se ma bhen kar sakte hai
 then ab sari command se ke allias set karna ho toh
 opt me jao 
 cat  grc.sh
 then copy kar lo ise /etc/ ke andar
 ls -lh se checck kar lo file aai ki nhi 
 then profile.d ke andr grc.sh file hai use check kar lo
 then ~/.bashrc
 export ke ek line chodkarr
 GRC_ALIASES=true
 [ -s "/etc/profile.d/grc.sh" ] brackets ke andar && source /etc/grc.sh 
 then save and exit ab login logout ke bad bhi colour aayeega
 # yum install zsh* =
 then yum search zsh 
 then shell ke andar entry aajayegi
  echo $SHELL
  zsh
  ab wo configure nhi hogi toh use hume configure karna padega
  nano /root/.zshrc
  ab khali hogi use bharni padegi
  then kali ki zsh waha place kar denge
  fi ke bad GRC_ALIASES=true
 [ -s "/etc/profile.d/grc.sh" ] brackets ke andar && source /etc/grc.sh 
  kali ke home location pe show hidden files .zshrc
  copy karo paste karo .zshrc me then save and exit
  then connection close karke waps banao then zsh shell
  then command doge toh kali ki tarah chalega
  then ab shell login ke doran hi chaiye toh 
  which zsh
  then chsh -s $(which zsh ) 
  then passwd me root ka shelll change dikhega
  # simplw way of this -
  SHELL

BASH (GRC) 
 	 
 	wget https://github.com/garabik/grc/archive/refs/tags/v1.13.tar.gz

 	tar -xvf grc-1.13

 	cp -vr grc-1.13 /opt/  (from ~ location)
s

 	./install.sh (in /opt/grc-1.13/)

 	cp -v /opt/grc-1.13/grc.sh /etc/

 	vim ~/.bashrc 

 	GRC_ALIASES=true
	 [[ -s "/etc/profile.d/grc.sh" ]] && source /etc/grc.sh 
do brackets hai aage

	source .bashrc 

	(bash shell will be colorful)

	------------------------------------------
ZSH SHELL

	yum install zsh*

	kali ki zsh copy karna heee

	vim .zshrc (copy the content of kali zsh) 

	GRC_ALIASES=true
	[[ -s "/etc/profile.d/grc.sh" ]] && source /etc/grc.sh 

	source .zshrc 

	chsh -s $(which zsh) or vim /etc/passwd /usr/bin/bash-zsh

	ping 8.8.8.8 / ifconfig