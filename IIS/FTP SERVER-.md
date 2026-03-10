 #concept --yadi koi server hamara dusri country me place hai hume us tak file  bhejni hai toh hum pendrive se toh nhi le jayengee toh yaha file transfer protocol work karega
ab koi website hamari dusri jagah place hai ab hume koi file us server tak phuchani hai toh by ftp bhejenge
![[Pasted image 20250205120245.png]]
# practical portion-
add ftp server on server hand in webserver section both options
iis ke andar ftp server aayega
# anonnomos site build-
ab ek server pe humne files rakh di ab sab use download karpai through ftp=
site pe right click add ftp site
physical path- c -inetpub-ftproot
default code server side 21
no ssl 
ftp- work to transfer file outside the lan
samba- in the lan file trrasfer (cifs)
ab koi bhi content jo kisi ko dena hai toh wo content ftproot me rakh denge 
aur by file explorer jisko bhi chiaye data access karenge
ftp://ip
by file browser aur by filezila use kar sakte hai
filezila ka cllient le lo
if anonymous give ip and username me enter password bhi nhi port bhi nhi
read ki permission hai toh server se data client me la sakte hai client ka server me nhi iske liye write ki permission deni padegi
# basic site build-
ab kisi company user ko hi access dena hai toh basic de do ab wo id password mangega 
read write dono ki permission de do
ab ftp se write ki permission toh de di company user ko but folder me ntfs nhi di 
ab un company user ko permission de do un folder me write ki permission ki
by filezila ip dena paddega username aur password bhi
# by cmd access-
#anonymous - ftp ip
anonymous user se unki gmail mangi jati hai
in anonymous three usernaame take- anonymous/ftp
password jo chahe wo do'
? marks se commands dekh lp
get se server se data lenge aagee file name
put se server me rakhenge aage filename