# umask value-
used for security in linux
when linux make file they give permission of 666 to files
and 777 to directory
but in this case user have full permissions to change folder 
thatswhy umask value is used for enhance security 
 default permission - umask value= permission to linux folder
 umask  command se pata lag jayega  umask value
 change karke dekh lo 
 then make folder and file permissions me chnages milega
 ab kisi file ka owner root hai toh root use modify kar sakta hai but dusre user nhi karsakta jab tak uske pass write ki permissiom na ho
 change owner karke bhi dusra user us folder ya directory me chnages kar sakta hai
 command - chown ownername file\folder name
 ab root user hi permission change kar saakta hai 
 bhalai dusre user ka wo khud owner kyu na ho wo ownership chnage nhi kar saakta
 ab jaise ek folder hai r1 nam ka uska owner root hai aur uske andar d1 and fil1 nam ki directory aur file hai aur unka owner rohan hai tab bhi rohan unko delete kar nhi sakta kyunki wo r1 ke andar hai aur r1 ki ownership root ke pass hai
 infact parent folder ki permission sab par lagu hogi
yadi root r1 ko delete karna chahe toh nhi kar saakta because of d1 and f1 ki ownership rohan ke pass hai
empty dierectory aur file hata sakta hai root
AB jaise r1 ke andar d1 hai aur d1 ke andar bhi koi file hai toh use sirf d1 hata sakta hai r1 bhi nhi kyunki wo data ka parent folder d1 hai 
chown - change owner 
[-R] se  directory ke andar ki bhi files ka owner change ho jayega
-[f] chupke se hata dega
-[v] bata ke pura
wildacards chalte hai isme
:groupname se group change 
[-R] group me toh wo group ke jo bhi member hai unki bhi ownership change
  ![[Pasted image 20250108185358.png]]
  change group command -
  ![[Pasted image 20250108190010.png]]
  same switches like change owner
  