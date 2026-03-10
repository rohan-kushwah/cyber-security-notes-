grep second
1. sabse pehel word dhund lo 
2. first me dhundna ho toh "(^)"
3. last me toh "( $)"
4. bich me toh "(&)"
#empty lines lani ho toh-
1. vim kara os release ko empty lines banai humne  
2. the command grep "^$" then file name
3. nhi lana ho toh -v
4. # for commented line
5. commented line lani  h  toh grep '#' /etc/ssh/sshd_config
6. -v se commented line nhi aayegi 
7. ab khali empty lani ho toh grep '^$' file name
8. sath me empty lines bhi hatani ho toh grep "^$" file name | grep -v "#"
9. or grep -E -v "(^#|^$)" /etc/ssh/sshd_config
then make a file 
user and number do
1. word dhundo
2. single character
3. double  character
4. / 
5. case insensitive -i
6. then give number
7. then give range of numbers
8. digit wala main [0-9] single digit
9. [0-9] [0-9] 0-9 double digit   
10. 4 ke bad single digit chaiye toh [0-9]
11. alphabet chaiye toh [A-Z] 
12. -I FOR CAPITAL SMALL
13. user ke bad three digit user 3 times [0-9] 
14. then passwd flle me jake dekh lo 1000 ke upar chaiye toh
15. grep 1 [0-9] [0-9] [0-9] [0-9] [0-9] 
16. -o value or pattern jo match hota hai wo lake deta hai
17. current location pe match karna hai toh command 
18. grep root *
19. pure etc me root dhund ke dekh lo
	1. grep root /etc/* 
	2. error aai toh /dev/null me redirect kar do
	3. then -R recursive directories me bhi dhundega
	4. -n for   line numbers
	5. -i for capital small 
	6. -r like recursive but  ye symbolic links nhi dhundta
	7.  -l list of file name jha match ho ra
	8. then if config
	9. init dhundo
	10. ip6 nhi chaiye toh space de diya 'init '
	11. -v 6 ke alawa
	12. ps -aux running processes bataiga
	13. yaha root grep karo
	14. session dekh lo