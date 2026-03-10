jaise hum kisi bhi binary ko setuid ki  permission dete hai toh ab wo jha bhi run hogi root i ya owner ki permissions se run hogi
ex- humne id command pe setuid laga di ab wo jha bhi run hogi root ki power se hogi
ab hum ye chate hai ki wo binary root ke alawa kisi particular usr  ya group ki power se temporary   run ho toh usme hum sudo ka use karenge 
ex - sudo -u rohan -g emp id ab ye id command temporary is permissions ke sath run hogi
yadi permanent kisi user pe wo binary ko run karna hai toh Vim /etc/sudeors me entry kar do
#kisi user ko sudo  chalani ho toh pehele us user ki entry sudeors file me karni padeagi 
like kushwah ALL=(rohan:rohan) /usr/bin/id ab kushwah user me sirf id command aisi hai jo rohan user and group se chal sakti hai baki koi se bhi user se  nhi by sudo 
sudo -u rohan id only'
# basic=
sudo --help
se switches dekh lo [-u] se user ki permission se run hogi [-g] se group ki permission se
root  ALL(ALL:ALL)  ALL
root hostname (username:groupname) commands 
hostname - means kisi bhi pc pe wo user us command ko run kar sakta hai
root - ek user hai
username - kisi bhi user ki power se chalani ho 
groupname - kisi bhi group ki power se chalani ho
commands - konsi command chalani hai 
sudo -l se dekh lo us user ko kya kya powers hai aur konse group aur user ki power se
Vim /etc/sudeors
isme changes karne padenge
[-i] se direct us shell pe le jata jis shell se use power di hai run hone ki command dene ki jarurat nhi 
[-b]  se background me place karega
[-s] for shell defiend
# echo $path=
path variable define for = jo bhi binary run ho rhi hai wo apke system me kha place hai
wo whi se utha ke deti jab hum kisi binary ko run karte hai
which se check kar lo
hum Vim /etc/sudeors file me jab changes karte toh puri location dalni padti hai command ki
sudeors me last lines dekhenge jo uncomment hai
# % wheel -
se yadi hum is group me kisi bhi user ko add karde toh uske pass sab permissions aa jayegi har commands ko chalane ki passwd me dekh lo 
then group me
add karna ho user ko toh gpasswd 
wheel group me password set kar do toh jisko password pata hoga whi us group me add ho sakta hai by [switch group command]
# for root=
rpm -qa | grep sudo
install hai ki nhi
then nhi hai toh yum install sudo 
list of file of sudo = rpm -qa sudo
jaise hum kisi user se login hai aur hume dusre ki home directory me file banani hai but ab jab file banegi uski permission me user aur group jo user login hai uske hisab se aayegaa hum chahte hai ki jiski home direcctory ho uski permissions se aai to hum hum jab bhi mkdir chlainge us user per toh wo group aur user me jo set hai uski hi dega
sudo -u rohan -g emp mkdir toh wo iske way me hi group and user dega us directory ko
sirf ek bar karega ye sab har bar sudo karna padega
ab ye root ke liye tha 
imp - wo directory or file me hum another file bana re sudo se toh wo file ka owner wo hi hona chaiye jis user ki permission hume us user pe chaiye
# for other user -
hume   Vim /etc/sudeors pe changes karne paddenge ki ye user is binary ko falani group aue user ki power se run kar pai
particular user ko kisi particular  command ki permission se run karna ho toh wo bhi kar sakte hai
imp - wo directory or file me hum another file bana re sudo se toh wo file ka owner wo hi hona chaiye jis user ki permission hume us user pe chaiye
jaise hum root se login hai but hume kisi user ki home diretory me uske behalf pe file banani ho toh hum sudo ka usse karenge
sudo -u rohan mkdir d1 
toh ab wo rohan ki behalf pe root banaiga
