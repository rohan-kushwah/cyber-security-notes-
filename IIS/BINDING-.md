FOUR TYPES OF WEBSITE BINDING-
1. by ip
2. by port
3. by type
4. by name 
#basic- sabse pehle template ko extract karo the unhe place karo inetpub me root me
ab hum kisi ko sirf admin hi panel dikhan cahte toh ip ke sath folder location hi de do
127.0.0.1 local pc se hi wo website tak ja sakti hai
# by ip- 
go in  iis option
    give multiple ip to server or client toh ab ek hi website alg alg website se call hogi 
     multiple ip - ncpa.cpl-properties-ip4-advanced-add
       #unstandard binding-extract template folder ko inentpub me dal do toh ab ip ke aage folder name de do toh wo website laiga
    for privacy in website only company wale khol payega 
    
     #standard binding - 
    in iis sites- add website
     then site name and site location inetpub se

     * * hai toh sabhi ip se whi website chalegi 
     * http se 
     * port 80
     then us ip se ab ek particular website chalegi
# by ports-
     single ip se alg alg port pe alg alg website chalani ho toh ports alg alg de do
    we have 65536 ports use them
     http 80
      and https 443
    then give ip:portno
# by type-
       2 type
    1. http
     2. https
    first iis me jake server certificate le lo then - create self signed certificate
      then ab binding me https me new binding kar lo with ssl certificate
    combination bana lo port type aur ip ko
    https://thenip
     then ab ek hi website ko dono type pe chalana ho toh jo website hai uspe click   binding me add kar lo
   doubt- multiple website ek ip se = alg alg port se bind kar do
     ek ip se multiple website - port se
# by name=
     by name ke liye dns jarruri hai  
     pehle zone banao  
    unke a records banao  
    then sab ke c name   
    ab nslookup karke zone check kar lo  
    then ab iis me jake add karo us ip se karna hai jisse humne us zone name ko bind kiya hai  
     then jab bana lo site toh binding me jake alias aa kar sakte hai  
    https se bhi bind kar lo  

    same ip se alg alg website - by name bhi hogi but alg alg ip ek zone me rahenge toh wo 'jo binding ki ip hai uspe rich nhi karega' toh dikkat aayegi wo website alg alg ip pe jayegi isliye unho hata do jab alg alg website same ip se by name call hogi  
    ab kisi pc se wo website call karni hai toh hume server ka dns dena padega  
    bina dns diye krna hai toh client  se dns hatao 8.8.8.8 do  
    aur phir host file me entry kar do client ki with website ip and zone name
    host file-C:\Windows\System32\drivers\etc