awk command
horizontal aur vertical word ko dhund sakte hai
advanced form of grep command 
awk pattern se word ko dhundta line by line
aur phir line me word match hogaya toh kya operations karna hai
particular line pe bhi action perfortm kar sakte hai bhalai usme pattern na match ho
khud ka syntax hai awk ka
 normal use casses
 awk '{print    $jis coloumn ko print karna hai wo number}'  file jiski lines field karni hai
 line o denge toh puri lines print kar dega [coloumn wise] 
 then define field seperator
 ex - awk  -F:  '{print $0}' /etc/passwd  
 then column ki valaue change karke dekh lo
 range nhi aati isme
 multiple files bhi de sakte hai
 then pattern match kara ke dekh lo 
awk -f: ' /root/{print $0}' /etc/passwd 
then jaha match hua uski field no 1 2 3
then ifconfig me karke dekh lo 
print wagera inet ko match karwa lo
mtu print karwa lo
# structure of awk command
	awk 'begin {code in begin section } {code in main body section } {code in end section} input file.txt
	begin part aur end part output ko decorate karne ke liye use karte hau
	main body ke bina awk command nhi chalegi
	example of awk structure - ifconfig | awk 'begin{print " ==ip=="} /inet/{print $0} end{print "-ip-"}' 
	echo 1234 in words
	then echo123 | awk 'begin{print demo}{print demo1} end{print demo2}' 
	ab echo wali command hata ke
	the ab output nhi ayega input file deni padegi
	etc/passwd
	jitti line utti bar print karegaa
	being part akelaa chalta hai end bhi
	sath me bhi chalte hai
	then tino part passwd ,e bhi karke dekh lo
	# print jabhi karega tab proper syntax ho double course
