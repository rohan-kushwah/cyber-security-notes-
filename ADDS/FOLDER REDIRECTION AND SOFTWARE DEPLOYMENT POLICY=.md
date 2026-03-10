client ka data server pe dekhna ho toh is policy ka use karte hai
server pe jao folder banao uske share karo everyone ko with read and write permission the
jis client ka data centralized karna hai us pr folder redirecction policy lagao user configuration - policies-windows setting - folder redirection policy 
wha se jo bhi folders dikh rhe hai unko basic kar do  
then jo folder share kiya hai uski network location cmd me jake \\ip of server folder jo share kiya hai uski location root path me dal do
then gpudate /force
then ab jaise hi user login hoga uski profile server me banne lag jayegi pehle jo sirf client me banti thi wo  
ab us pc me local hardrive pe bhi prevention laga do
ab jo bhi data wo save karega wo network pe hoga  
jha jha wo device login hoga use milega
aur server ke pass bhi rahega
jisme data rakh ra wo user usko local admin se login karke permission full control  ki server ko de do toh wo bhi dekh payega
user use delete bhi kar sakta hai
ab then local admin ban karenge toh data koi nhi le ja sakta kyu ki wo network par  hai 
# software deployment policy -
ab admin chahta hai ki koi particular ap uske user ke pass ho toh wo is policy ka use karta hai
pehle toh koi bhi app user ke control panel ke network me nhi milega 
but ab hum server pe msi [microsoft installer wale app] download karenge aur share folder me dalenge
share folder ki network location nikalne ke bad app ka nam ki location
user configuration - policies - software setting - software installation 
right click file name me network address  de denge  
assign kar do ya publish
then gpupdate /force
ab application aa jayegi control panel ke program and feature ke network me
ab use install karenge toh run as admin hoga 
original locations pe install honge
ab w o use unistall nhi kar sakta jab tak server ne nhi hataya ho toh jha se policy  lagai thi wha jake apps ko right click karke remove kar do