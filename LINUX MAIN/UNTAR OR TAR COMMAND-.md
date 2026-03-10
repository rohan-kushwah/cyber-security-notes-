first untar after bzip and gzip ek sath nhi ho ra  -command chaala ke dekhni hai
1.text editor command-
1. we use cat for text editor like read or write 
2. cat file.txt for read 
3. cat file.txt > file.txt for write 
4. isme ek dikkat hai shuru me hame kisi command ke output me changes karna  hai wo nhi hoga 
5. ex - cat /var/log/messages 
6. hum cat se control c se bhar aa sakte hai
7. control d se shara data background pe place 
8. ek particular linee pe control d karoge toh connection close kar dega
9. jha control d kara tha us line pe wapas hume command panel dega us file ko read kar ke dekh sakte hai
10. control z process kill karta hai like control  c
11. dono command ke output ko ek single file me redirect kiya aur phir cat karenge aur use bhar aane ke liye switches ka use karenge uske karan  process bhi kill ho jayegi isliye hum us par particular extension laga deta hai =
12. cat /etc/shadow /etc/hostname > rohan.txt
13. ab hume particular jagah pe cat ko khatam karna hai toh matlab kisi word ke ate hi process band to hum-
14. end of file [eof] word define karenge 
15. ex -cat << kushwah>> file.txt
16. toh jab kushwah aayega toh ye process band kar dega
17. for the drawback of changes in first lines-
#we use nano command as text editor-
1. yum install nano
2. isme hum changes kar sakte hai 
3. control o se save control x se bhar
4. command aisi denge nano jis file me changes karne ho
5. sirf nano command di toh wo buffer memory me run kareng
6. isme bhi control x se bhar ayega
7. text karke file name deke  particular file me save kar sakte hai
8. control q kha pe jana hai wor dedo 
9. control r se ussi file ke andar dusra content bhi aa jayega
10. control k se cut u se uncut
11. control t se command chala sakte hai
12. isko bhi hum use nhi karte jyada settings ke karan
#end  