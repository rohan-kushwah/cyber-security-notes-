pehele  apn kisi employe ko data dikhana ho toh hum user banate the us pc par then uska id password hum employee ko dete the then wo data ko dekh sakta tha
then humne  user ko banana wagera server pe kiya toh data centralized ho gaya tha server pe ab but adds me administrator hi server ka data idhar udhar kar sakta hai 
then ab kisi company me do server ho  toh user ko kab tak ip ka yaad rakhega aur ksisi company me 10 bhi server ho sakte hai jinke pass files ho ab wo host ko chiaye ho toh use sab ke ip yadd rkhne padenge  
![[dfs.png]]

toh dfs jitte bhi server hai unhe merge kar leta hai then user server se file le paye
![[dfs conataineers.png]]
domain name de do / jis nam se container banaya hai wo
ab dono pc per data hai usse server ke jariye share karna haii toh dfs use karenge
server pe -
add roles and feature
file and storage services se 
 1. dfs namespace
 2. fsrm
 3. dfs replication
then dfs management
namespace then nameserver  me adds configure domain server ka name
then name of container you know
then admin ke pass full control
domain based me user server banaiga 
stan alone me user bana ke dena padega
then after name  server install  new folder 
then folder name do jo bana ho then jo pc se file uthani ho 
then jo pc ho usme jo share folder ho la skate hai
then name ko command line me browse karo sara control mil jayega \\ ke saath
then kisi pc pe jake bhi wo name do toh content dikhega sab
domain name k jagah server ka ip bhi de sakte hai
# fsrm file server resource manager =
ab server pe data rakhna hai toh user pura data hi share kar sakta hai toh ab server me storage ki dikkat aa jayegi toh hum space management karne ke liye fsrm ka use kar sakte hai 
#ouata - limit de deta hai ki itta data user rakh sakta hai
#file scanning - me konsi tarah ki file rakh sakte hai konsi hi nhi
ouata template me - kitti  limite ka data user server me rakh sakta hai
#hard qouta template me ek limit de sakte hai uske jyada data nhi rakh sakte hain
#soft quata me limit ke bad bhi data dal sakte hai but wo limit cross karne ke bad bhi data rakh sakta hai but admin se permisssion lena padega wo events me dikhegi
then ab ek particular group of user ke liye  folder banao server me the share ab wo users ko dusre pc ke login karo then network share me usme data dalo
then ab ouata pe jake create new qouata then jis folder pe lagana ho toh path me folder dekh lo 
then limit of data
then ab user data dalega toh jitti limit hogi utta hi jsyega
dono quota laga ke dekh lo
#file scanning extension per chalta hai=
ki ye folder me sirf yahi extension ki hi file hi place kar sakte hai
#have two parts - active aur passive
active me rakhne nhi dega data passive me rakhne toh dega but admin ko bataiga
then file screens me jake create  screen then jis folder me lagana ho laga do
photo nhi rakh sakte but uska extension hata ke rakh sakte hai photo bhi
then passive me rkh toh sakta hai but admin ko event me dikhega
