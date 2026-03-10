#character classes- use within bracket[]
1. [:upper:] bracket ke andar braket me - capital latter
2. [:lower:]bracket ke andar  bracket me - small letters
3. [:alpha:] bracket ke andar bracket me - dono letters
4. [:digit:] numebers
5. [:alnum:] alpha number dono
6. [:space:] special chracter like tab space new line
#ab wo user-number.txt file me use karo
with their main commands-
like-[A-Z] [a-z]  a-z 0-9  teeno saath me then space
/t for new tab
#ps -aux ke andar bhi commandds check kar lo
#make file of domain name text place domain name
#make url file placee url in it
domain and url ar related to each other
then hume domain find karni hai url file me 
means us domain ka url
ex- grep domian name url file name
ab sabhi domain match karni hi toh 
ex - grep -f domain.txt url.txt
-f for pattern of file
jha match nhi hua wo particular domain chaiye toh -v
# cut command
filter row wise
1. csv file banao 
   2. ex -cut  f1 csv.txt
   3. but syntax nhi lega then set dlimitor 
   4. ex -cut -d '.' -f1 csv.txt first colomn
   5. ex -cut -d ',' -f2,4 csv.txt
   6. ex -cut -d ',' -f3-4 csv.txt
   7. jo output aa ra use save karna ho toh particular file me redirect akr do
   8. same passwd file mee nkar do 
   9. syntax ':'
   10. then in ifconfig
   11. pehele horizontal inet ko grep kar lo through grep command
   12. then ab vertical through cut command 
   13. ex -ifconfig | grep inet | cut -d '  ' -f10
   14. ab interface name chaiye toh
   16. ifconfig | grep -v '(^$)' | cut -d ' ' -f1
   17. 