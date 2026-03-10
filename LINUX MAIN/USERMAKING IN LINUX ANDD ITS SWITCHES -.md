history ek shell ki dusri me dalna ho toh cat ~/.bash_history >> ~/.zsh_history
jab bhi user add karenge toh passwd /group/ shadow / gshadow charo saath me update hogi
tail karke last ki five line dekh lo
# user add and its switches -
adduser username
useradd username
dono se add kar sakte gai users 
then user add karenge toh sari files ek sath update hogi
adduser --help se sare switches dekh sakte hai
home location pe user banai uske bad entries dekh lo aai ki nhi
home location ko customize kar sakte hai -
by - cat  /etc/default/useradd 
changes karna ho toh jo line ko change karna hai uske aage # then new line me entry kar do save and exit
then ab jo bhi user banainge jo changes kiye hai uske sarh banega
then phir change ho jayega jab dusra user banainge toh
home location create karna hai user ki toh - [ --create-home then username] 
[-d] kha karna hai home directory create 
[-m] home directory create karna hai
[-M] directory nhi banegi
[-c] for comment " likhna hai comment"  in passwd file
[-N] se user toh baneega par uska group nhi banega
[-s] shell change then shell name from shell file and username
[-u] se uid change kar sakte hai
ab same uid kisi user ko dena ho toh [-o] ka use karenge
[-g] se gib nai nhi de skte hai but new user me purani gid de sakte hai
then [-p] for password set  ke liye new  user me
ab wo password set toh ho gaya but uska encrypted way chaiye toh ab uska 
kyuki wo passwd file me without encrypted password dega
encyrpted version dekhenge
openssl passwd 123
toh 123 ka encrypted version de dega
then use place kardo adduser -p "encrypted password" username
then wo passwd file me # adha hi ayega
toh jha bhu doller dikhe uske aage \ laga do
then ab usse user jo banaya  hai wo login hi kar sakta hai
# password genertor $6 $ SHA512
openssl passwd -6 then password
toh sha512 me encrypted password dega
ab wo wese hi daldo 
adduser -p "encryted pass" username
jaha $ ho uske aage \ 
then isse bhi login kar paoge
# tool for password genrator- 
yum install expect 
then yum install mkpasswd
[mkpasswd -m sha-512]
then password
toh uska incryption aa jayega
then 
[mkpasswd -m md5] 
passwd 
toh uska md5 version aayega
jo bhi user banao aur password set kare toh use login karke dekh lo'
then sari command ek user pe ek sath chala ke dekhlo
[-G] se secondry group add hota hai 
useradd -m -d /home/armour1 -s /bin/bash -G secondry group name then username toh 
/etc/group  me jake  us user ka secondry group bhi dekh sakte hai
#system user bana ho toj =useradd -r sys_u1
[-e] se us user ki expiry set kar sakte hai
