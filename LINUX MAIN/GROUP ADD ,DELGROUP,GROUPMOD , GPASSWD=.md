sabke pass help switch hai dekh lo=
![[useradd 1.png]]
group add se group add karenge 
jab user banate hai toh uske sath sath group bhi bante hai wo group primary group hote hai baki secondry
pehele it,sells,emp,hr group bana lo unme user add kar lo then hr ko un sabme addd kar do aise user add karna jinpe password ho
then [-g] se gid change kar sakte hai 
pehle se group hai aur uski group id dusre ko deni ho toh [-o] ka use karenge
then jo group banaye hai uspe password set kardo 
openssl passwd then password
then [-p] for password set in group 
gshadow me passwords dikhenge
[-r] se system groups banenge
# group mod -
[-g]
[-n] for rename the group
[-p]
groupdel se  group delete ho jayega'
# adding of users in group=
usermod -G then secondry group name primary group name BUT ISME WHI GROUP ADD HONGE JO PRIMARY YA USER BANATE HAI USKE SATH HO
then gpasswd -a  username group name
toh wo user is group me add ho jayega
-[a] se ek user add hoga [-M] se multiple users add honge
groups then username toh username kis group se join hai pata lag jayega
aur kisi particular user pe direct groups command dege toh pata lagega wo user kis group ka member hai
group ki entry gshadow me dikhegi
[-A] se administrator bana sakte hai kisi user ka 
gshadow me dikhega
[-d] se user delete ho jayenge group se
gpasswd -d username groupname
ab admin bana ke user ko hataoge toh wo admin raahega
[-r] remove the passwd of group
[-R] ME wo user ko restrict kar dega add karne se
in id command gid me jo output milta hai primary group baki secondry
