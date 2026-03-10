GREP AND CUT ADVACED
IFCONFIG ME INTERFAACE GREP KARNA HAI TOH JO INTERFACE LINE ME COMMAN WORD HAI GREP KAR LO
EX- IFCONFIG | GREP FLAG | CUT -d ' ' -f1  
then netmask grep kar ke dekh lo 
ex- ifconfig | grep 'net'  | cut -d ' ' -f13
then make a file  with find command find / and redirect its output in
fileindex.db
cat fileindex.db 
ab hume is file ke andar se kisi file ka data dekhna ho to 
hum grep cut sort commands use karenge 
ex  -var ke andar ka log ke andar ki files dekni ho
ex-cat file.index.db | grep \/var\/log
slash aara toh usko ulta slash deke katenge
ab slash ki files dekhni ho to tph
cut -d '/' -f1-3 fileindex.db | sot -h | uniq
cat fileindex.db | grep'^\/' | cut -d '/' -f1-3 | sort | uniq
ab home ke andar user dekna ho toh
cat fileindex.db | grep '\/ home' | cut -d '/' -f1-3 | sort | uniq
yaha f range se folder ke andar folder khol sakte hai
then check same in var /log
then particular extension wali file chaiye toh wo direct grep se ho jaayega
grep '/.txt' fileindex.db
end me match karwa ke dekh lo
conf dhund l0 exe dhund lo html
js dekh lo
then ps -aux ke andar dekh lo root ke sessions wagera
root wale nhi chiye toh -v
the ab root ke alwa jo aa re unko grep karna ho toh
ps -aux | cut  -f1 -d ' ' | sort | uniq -c se konse user se kitti process chal ri bataiga
ab user nahi chaiye toh use grep karke hata do
then we have paste command   
   1. do file banao haave namee and surname
   2. ab dono ko series wise jodna hai toh paste
   3. paste file name file name
   4. the use delimitor for space underscore - etc
   5. ex  - paste d ' ' fil1 fil2
   6. #end 