sed  {stream editor} command 
used for - text replacing and finding of text mainly
particular line pe jump karna ho kar sakte hai {main motive}  
#syntax-
options = switches
script = main body
input file
ex = print particular line 
sed -n  '1,5p' /etc/passwd
ab ye passwd ki1- 5 line print karega
comma yaha range ke liye use hota hai
-n used as switch
p for print in main body
after lines we have single letters command like {a,p,d,s,q,i} 
for deleting the line
sed -n  '1,5d' /etc/passwd
backup bana ke use karna'\
[-n auto print ko rokhne ke liye]
ab empty line banao kisi file me aur hume empty line print karwana ho toh -
sed -n '/^$/p' passwd
ab empty nhi chaiye toh -
sed -n '/^$/!p' passwd 
then empty lines ko hata do
bich me se bhi line ko print karwa sakte hai
ab kisi particular line se last tak print karwana ho toh
sed -n  '1,$p' /etc/passwd
#then for condition pattern-
  sed -n '/bash/p' passwd
  jis line me bash ho wo print karo
  jha condition match hoti ho uske age ki do aur piche ki do line print karo
   sed -n '/bash/,+2p' passwd
  sed -n '/bash/,-2p' passwd 
  jha bash ho use delete karna ho toh 
   sed -n '/bash/d' passwd
  #replacemet of text=
  sed -n 's/bash/BASH/P' passwd
  BASH KI JAGH base aa jayega ab wo line me first bar aa ra use hi replace kar ra sab jagah karna ho toh [g] 
  sed -n 's/bash/BASH/Pg' passwd
   sed -n 's/bash/BASH/1P' passwd
  1 diya jab pehli bar bash ayega toh replace karega
  #particular line me changes karne ho toh--
  sed -n '4 s/sbin/SBIN/pg' passwd
  then range me bhi replace karke dekh lo
sed G file.txt 
space de dega har line ke bad
sed G;G file.txt 
do space dega

  
  