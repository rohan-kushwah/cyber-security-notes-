#copy command  and wildcard
  1.*          any number or character including zero
  ex = ca*
  cat car carpanter etc.
   2.?       single character 
   ex= hel?
   help hell  
   3.[]        single character from range  file[0-2]
   ex= file1 file2 file3
   4.[!]    single character file not listed in bracket 
   ex= file[!1]
   file 2 file 3
5. { }  comma seperated terms 
   ex={*.log , *.txt}
   hello.txt  comma.log
   follows particular pattern of all
   group of pattern
# COPY COMMAND 
IMP.POINT = SOURCE MULTIPLE HO SAKTE HAI DESTINATION EK HOGI
EX      ==CP   JHA PE KARNI HO FILE COPY    AUR FILE NAME JISKO COPY KARNI HO

JO ANKHIRI ME AYEGA WO DESTINATION BAKI SHURU KE SHARE SOURCE HONGE
EX=cp -v rohan rohan hardia  kushwah  d5/
 # multiple files sirf ek folder or directories me hi copy hoga
#  single file ko copy karna ho toh hum kisi file ,me karr sakte hain bhalai wo file exists na karre
 in case of multiple file destination must be directory
 # current location pe ho aur wha copy karna ho toh direct . dot de do
  in case of copy of files **
  in case of copy of directories give r  recursive
  dd