archive or akay 
why we archieve files -
1. for make a backup for particular file 
#command - tar -cvf  [file name after archive file]   original file name for archieve
decribe- tar = archieve   c for create  v for vervus and f for forcefully

ex - tar -cvf messages1.tar messages1 
use wildcards 
also uses in directory for archive 
ex = tar -cvf log.tar log
  tar make bakcup   not compression of file
   #command = tar -xvf  file name jisko extract karna ho
   x for extract in this following command
   ex = tar -xcf log.tar
   #files ko delete  kar de uske  bad tar ko extract kar ke wapas la skate hai  
   ek extension ke andar maximum files ka tar rakh sakte hai
   #tar supports only bzip2 aur zip switch
    for both switches following commands are-
    1. for bzip=tar -cjvf messages.tbz messages
    2. for gzip = tar -czvf messages.tzg messages
 in tar 7z or zip not working
- hum kisi bhi extract kari hui file ko 
 #command = tar -xvf aur extract file se waps layenge
 yadi koi file ko compress kiya ho toh use by process se bhi  hata sakte hai 
 like bzip se kiya ho toh = pehle bunzip karo then tar se extracct
 #gzip tar file pe direct laga sakte hai 
 #du se kisi bhi files aur directory ke andar ka data dekh sakte hai 
 ex= du -h log
 #file kar ke hum uske andaar ki files details dekh sakte hai jaise humko kisne compressed ki hai wo dekhna hai to file command use karenge
 