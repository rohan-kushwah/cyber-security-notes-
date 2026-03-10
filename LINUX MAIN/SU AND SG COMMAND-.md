SG - SWITCH GROUP 

    The sg command is used to switch the user's group temporarily and execute commands as that group.

      sg groupname [-c command]

      sg developers -c "touch file.txt"


SU - SWITCH USER

    The su command is used to switch to another user account, optionally starting a new shell with their environment.

      su [options] [username]

      su username 

  Run a command as another user:

      su username -c 'command'
# switch user-
from root su username
koi password nhi mangega
  ab su username se hum home directory pe uski nhi ja skate ls aur id se dekh lenge
  but
  [-] or [-l] or [--login] se hum proper login honge us user pe id ls bhi chalegi wha
  aur log me entry bhi hogi
  [- c] se command chala sakte hai
  su username -c 'command'
  dusre user se switch user karoge toh password mangegaa
  jha bhi user define nhi karoge toh by default root se chalaiga
  shell bhi define kar sakte hai
  su - rohan -s /usr/bin/zsh
  toh wo rohan user zsh shell se login hoga'
# switch group=
temporary member of the certain group
root kar sakta hai direct 
sg --help
root se sg groupname jispe switch karna hai
id me dikh jayega primary group change ho jayega
exit command se purane group me wapas aa jayenge
yadi normal user us group me switch hona chahta hai toh us group me password set hona jaruri hai gpasswd se karo
change karwane ke bad id chalata jao
change hone par primary me dikhega ab us user ke pass us group ki power aa gai
[-] lagane se primary group add kar rha hai
baki me secondry
yaha bhi -c hai
aur ex sg hr 'id'
ab wo id command chala ke bhar ho jayega group se
su se bhi group switch ho sakta hai
[su - armour -g hr ]
[su - armour -g hr -G it2]
ab it2 secondry group me jayega
ye sirf root hi kar skta hai
   
      