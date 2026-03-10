# STING PROCESSING 
PARTICULAR OUTPUT KO APNE MARZI SE FILTERR KARNA 
ex- lines dekhni hai shuru ki last ki  jo process hoti hai last ki jo last me write hoti hai usko bhi write karna
#head-
1. command hume shuru ki 10 line la ke  deta hai 
2. ex-head /var/log/messages
3. maximum file bhi support karta hai 
4. ex= head /etc/shadow /etc/gshadow /etc/passwd 
5. switches - head -n line kitti dekhni hai wo /file name jiski dekhni hai 
6. ex- head -n 4 /etc/shadow
7. character dekhna ho toh [-c] 
8. ex-head -c 50 /file name jiski dekhni hai
9. multiple files me header dikh jate hai
10. single files me headers nhi dikhate hai isliye hum [-v] ka use karte hai 
11. ex - head -v -n 5 /etc/passwd
#tail command-
1. last lines dikhata hain 
2. use for bhut si commands data ko lasts se save karti hai toh use read karne ke liye  tail use karte hai
3. supported maximum files
4. . ex= tail /etc/shadow /etc/gshadow /etc/passwd 
5. switches - tail -n line kitti dekhni hai wo /file name jiski dekhni hai 
6. ex- tail -n 4 /etc/shadow
7. character dekhna ho toh [-c] 
8. ex-tail -c 50 /file name jiski dekhni hai
9. multiple files me header dikh jate hai
10. single files me headers nhi dikhate hai isliye hum [-v] ka use karte hai 
11. ex - tail -v -n 5 /etc/passwd
12. we have [-f] switch for single file updation in data bataya karta hai
13. ex -tail -f  /var/log/messaages
14. connection break or login karenge toh hume changes milega data me
#wc-word count-
1. word counting karna hai  toh use karege jaise
2. ex -wc /etc/passwd
3. maximum files supported 
4. ex -wc /etc/passwd /etc/groups /etc/shadow /etc/gshadow
5. orde -line word character
6. wc -l line
7. wc -w word
8. wc -c character
# sort full switches and pattern
define-- for manage data in particular way 
 vim use kana hai
1.command sort file name 
2.place data in it 
3.give sort command
#switches of sort command-
1.sort karenge toh first letter ke behalf pe sort karnege
2.[-h] for human readable 
3.ex -sort -h file name
4.[-d] for dictionary ke acc
5.[-n] numeric sort number pehle character bad me
6.numeric aur human sort same hai
7.[-r] reverse short ulta dikhaiga
8.[-u] uniques sort ex - sort -u -n file.txt
9.toh wo number ko chodkar sabhi ko sting bana dega
10.[--uniques] same as unique sort
11.[-R] random sort kahi bhi rakh dega no pattern apllied
12. alg file li age aur name ke sath me data place kiya to hume kisi particular user ke basis me data ko jamana hai toh
13. ex - sort -h -[k] 2 file name jiska data karna ho
14. ex - sort -h -[k] 3 file name jiska data karna ho
14 . for comma seperated value[csa]  like  = name ,age ,work
15 . ab isme age ko [-k]se sort nhi karega because of gap
16 .then hume [-t] aur ',' use karenge like
17 .sort -h -k 3 -t ',' file name
18 .then again make file  month-name.txt
19 . month sort karna hai we have [-M] 
20 . then for duplicates file 
21. [-u] uniques karega
22. [uniq] duplicates to sort karegaa nhi isko humko 
23. sort karne wala data dena padega
24. ex = sort g.txxt  | uniq
25. [-c] for character kiti bar aaya hai wo bataiga
#watcg commandd-
define - particular time me command ko run karna
ex - watch -n 5 command name
w karke dekh lo 
then band karo chalu karo connection ko changes dikheneg

