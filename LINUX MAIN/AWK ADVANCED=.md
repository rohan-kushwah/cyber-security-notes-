awk advancesd
begin print akeli chal jati hai end aur main nhi chalti without use of command
#special characters- 
echo 1 2 3 | awk '{print $0}'
echo 123 | awk '{print $1,$2}'
ab  $1 aur $2 ke bich me special kuch chaiye toh 
echo 1 3 | awk '{print $1 "-" $3}'
toh - ha jayega 
space dekh le relatable word s dekh le character dekh lo etc
 d limitor ya file separator ap nhi de rhe hai toh wo space count hoga
 then ye passwd file me karke dekh le
 #change the value
 ex - echo "armour infosec" | awk '{print $0}'
 ye pura layega 
 ab ek field print karenge toh
 toh field 1 , 2 kar do
 ab value update karni hotoh
 echo "armour infosec" | awk '{$1="Armour" ;print $0}'
 then ab armour ki jagah ARMOUR aayeega 
 file me bhi changes kar sakte hai aur multiples column par bhi 
then ifconfig karke inet ko apni way se grep karo
ifconfig | awk '/inet/{print $2}' 
or aise bhi kar sakte hai rasta bata ke
ifconfig | awk '$1=="inet"{print $2}' 
condition field line 2 lao jiski field line 1 = inet ho
mtu netmask ether karke dekh lo
then passwd file ke andar root ko grep karke dekh lo 
awk -F: '$1= " root"{print $0}' /etc/passwd
aisa line print karo jiski pehli fied root aa rhi ho
then rohan wali line find kar lo
6th field pe home location and 3rd pe uid
jis user ki uid 0 wo root baki other
awk -F: '$3=0 {print $0}' /etc/passwd ek aur = aayega
not equal zero karwa lo greater karwa lo less karwa lo
#kisi file se bhi pattern de sakte hai
pattern de do file me
then -f laga ke jis file me pattern diya hai wo 
ex =awk -f file.txt -F: /etc/passwd course aur ka dhyan rakhna
 #mathematics calculations bhi kar sakte hai
 ex -echo 2 4 | awk '{print $1+ $2 }' 
 multiply plus minus karke dekh lo
 #end 


