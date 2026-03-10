Dpkg" stands for "Debian Package" and is ==the core package manager used in the Debian Linux distribution and its derivatives like Ubuntu==, essentially allowing users to install, remove, build, and manage software packages on their system through the command line, primarily working with files in the ".deb" format; it's considered a low-level tool, meaning other programs like "apt" often sit on top of it to provide a more user-friendly interface for package management.  


use in debian based operating system
used as package manager in debian like operating system
.deb file system used in debian like operating system
os -relaease file check kzr lo
# commands-
     #dpkg --help
     
     #dpkg -l = all packages list use wc -l
     
    #dpkg -s package name == query package ki     
    
    #dpkg -L package name== list of ffile of packages
    
    #dpkg --contents package name  ===  for non install packages query
         or 
         dpkg-deb -c then package name  same output
         
     # first apt-get -f install for fix broken
     
    #dpkg -i package name == installation of package  u
   
     only install for local users 
      
      aur koi dikkat aai toh fixed dependecies
       apt-get -f install
          apt --fix-broken install
          
    #dpkg -r package  name adaha hi  == remove of package 
    
    #dpkg --verify package name == install package ko verify karega
    
    #dpkg -p then package name == sabhi files hataiga , configuration file bhi hata dega
files gayab ho gai kisi package ki toh ab unhee wapas lana ho toh

    #dpkg-deb -xv then package name then jha rakhna hai wha ki location  
    
    #dpkg -S then file name - toh wo file kis package ke sath aai hai wo bafaigi
link hogi toh  dikkat aayegi toh link ke lasst tak jao phir dekho

    #dpkg --query package name - for query of packagees
    
    #dpkg -reconfigure packagenamee == reconfigur of installed packagees
    
    #dpkg --configure -a == package is an  bad consistent state
    
# isme dependencies ki dikkat aayegi toh hum apt use karenge

