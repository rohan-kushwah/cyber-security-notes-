user     ALL(ALL:ALL)  ALL
ek user all hosts me all groups and user ki power se all command chala sakta hai
Host_ Alias - SE KIS KIS HOST SE USER COMMAND CHALA SAKTA HAI [visudo] me entry karenge 
User_Alias - KIS KIS USER PE PE US COMMAND KO RUN KARSAKTE ho kis kis ki power se
Runas_ALIAS - SE KISKI PERMISSION SE RUN HOGI USERNAME KI POWER WALA
Runas_Alias - se kis group ki permission se run hogi group ko define karo toh % likho
[:] ke age groupname aayenge
Command_Alias - me konsi konsi command kis user aur group ki power se aur kis host se aur ki user se run hogi
alias se hum password aur no password bhi define kar sakte hai
:PASSWD for commands FULL LOCATION
:NOPASSWD FOR COMMANDS FULL LOCATION
CHANGES ON SUDEORS FILE
![[Pasted image 20250122002733.png]]
#jab tak user jispe hum group ki power se command chala rhe jab wo us group me add na ho tab tak nhi chalegi
ya jab hum useraliase banate hai usme wo user add karna padega jisse sudo chalani ho and run as me group name
![[Pasted image 20250122011515.png]]
ab jaise rohan particular user hai usko hum permission de re ki groupuser alias ke groups se usese hum run kar sakte hai but jabhi run hoga tab hum un group me is user ko add karenge 


#DOUBT- JAISE KUSHWAH USER HSI USKO HUMNE JO GROUPUSER NAM KA ALIAS HAI USSE HUMNE CMD RUN KARNE KI PERMISSION DE DI BUT AB WO USER KO ADD KARNA PADEGA UN GROUP ME TOH JAB ALL DETE HAI GROUPPOWER ME WO PHIR KAISA CHALTA HAI - anwer [wo jab alias banainge tb group name ke sath percentage nhi dena hai]