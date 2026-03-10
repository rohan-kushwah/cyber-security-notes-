 compress and archeive -
data compress kyu karte hai - 1. to make transfer speed fast and for space or storage
compression ka use karke backup bhi banate hai
1. install compression tool -yum install bzip2 then you have bzip2 tool for compression 
2. ab kisi bhi file ko compress karni  ho toh bzip2 ka use karenge 
ex= file copy messages make messaages version like cp  messages messages1 messages2 messages3 messages4
use of bzip2 tool-  
1. bzip2 karenge aur file name denge wo jo main file hai usee replace kar degi compress file se
2. bzip2 me wildcard use hota hai
3. bunzip2 se hum file wapas la sakte hai main file me
4. bunzip2 aur compression file ka nam
5. directory compression nhi hoti
6. log ke andar bhi hum compression kar sakte hai wildcard laga ke
#gzip 
1. install gzip command = yum install gzip
2. gzip bhi bzip sa hai gzip file name to compress
3. the gunzip to unzip the file name you compress
4. then use wildcards in message to compress the  wildcard extension file
5.  isse hum direct directory ko compress nhi kar saakte but hamare pass yaha  recursive switch hota hai jise folser ke andaar ka data compress kar sakte hai

#zip 
1.if not install install by command yum install zip
2.ye file ko replace nhi karta new file bana deta hai
how to command= zip file name jo compression hoke banegi  aur last me jo file jisko compress karni ho
3.unzip se file ko waps la sakte hai
4 . yadi  file ko delete bhi karde toh hum waps whi format me la denge
5  . zip me pura folder compress kar sakte hai  like zip -r  log.zip  log/
6 . compression of maximum file in onee file also supports in this

#7za 
1. install by command yum install epel-release.noarch
2. then after yes givee command yum install p7zip
3.  the help chaiye toh 7za--help
4. for compression 7za a  compression file name after compression  original file for compression
5. also compress directory
6. uncompres ke liye 7za e jo file jo compression ke bad bani
7.  . compression of maximum file in onee file also supports in this
8. unzip karne se pehle hum uske andar ka data dekh sakte hai by command = 7za  l file name jiske andar dekhna hai
9. also use in log ke andar sabko 
10. isse puri directory complete ho jati hai same command = 7za a  jo file ka nam banana ho   jo   directory ko compress karna ho
11. problem wo directory ko unzip karenge ko toh wo current location pe kar dega
12. kisi particular location pe karna ho toh command hogi = 7za e  jo directory ka data place karna ho -0    se directory ka nam jaha place karna ho
13. 7za t aur compressed file name usse hume headers wagera ki details milegi
