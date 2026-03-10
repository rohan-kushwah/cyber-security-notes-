#grep  command- particular word dhundna hai 
horizontal dhundega
 ex -grep word/line dhundna ho wo  konsi filee me dhundna hai
 #command ke output ko direct grape nhi kar sakte pipee karna padega
 ex- grep root  /var/log/etc 
 ex - grep root  /etc/shadow
 multiple files support-
 ex - grep root  /etc/shadow /etc/group /etc/gshadow
 ex- grep Root /etc/shadow /etc/group /etc/gshadow
 word dekh lo wc command se
 #[-i] se capital aur small dono dekh sakte hai 
		 ex= grep -i root /etc/shadow
		 or
#cat /var/log/messages | grep -i root
#running command me se grep nhi kar sakte hai
like- grep root ipconfig
#started session grep karna ho toh-
1. grep started session /var/log/messages
2. sirf wo started ko grep karega aur session ko file ki tarah treat karega toh error aayeegi
3. error ko hum output ko /dev/null me redirect kar ke dekh sakte hai
4. toh hum usee " started session" use karo toh koi dikkat  nhi aayegi
5. space ko cover hum / se kar sakte hai like-grep armour/ infosec
#alg alg words ko dhundna ho toh-
1. format grep -[E] " (armour|infosec)" /var/log/messaages
2. session bhi lana ho dono user ke toh -grep -[E] "(armour|infosec)" /var/log/messages | grep session
3. grep root /var/log/messages | grep session
4. root ke alawa kisi ka session dekhna ho toh 
5. grep root /var/log/messages | grep -v root
6. grep -E " (started session | starting session)" /var/log/messages | grep -v root
7. grep -E " (started session|starting session)" /var/log/messages | grep -vE "(root|rohan)"
#single character 
1. grep root. /etc/shadow
ab ye root ke badd single charcter ko dhundega
2. do lagaoge toh double character ko dhundega 
3. ab ek particular charcter hi chaiye toh 
4. / ka use karenge ex- grep root/. /etc/gshadow
5. toh . wala hi milega'
6. aur bhi nhi kam bana toh double course me likhenge
7. like "root/ ."
8. ab last me wo word match karna ho toh $ ka use karenge 
9. ex- grep "root/ .$" /var/log/messages
10. bich me dekhna ho toh - grep -v "rroot/.&" /var/log/messages | grep successfully
#starting me bhi match karwa sakte hai 
grep "^dec 4" /var/log/messages | grep session
upper case line ke starting me dhundta hai
1. grep "^dec 4" /var/log/messages | grep root
2. root ke session aa jayenge
