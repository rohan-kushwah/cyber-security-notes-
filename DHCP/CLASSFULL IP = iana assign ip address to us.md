 class A = 1.  one section network ka baki teen host ke 
class b =2 two section network ke and baki do host ke 
class c = 3 network ek host ka

#  range of ip in particular classes 
class A = 1.0.0.0 to 127.0.0.1
class b = 128.0.0.1 to 191.255.255.255
class c = 192.0.0.1 to 223 .255.255.255

ip kitte bit ka 32 bit ka
kitte section 4 
har section me kitii bit 8


127.0.0.0/8 loopback address  
255.255.255.255  worcast address
 gateway acc to you pc gateway 

configure dhcp server and give ranges of ip to automatically give ip to other devices in pc
how to discover -
1. go to server 
2. add roles and features
3. and add folowing required features
4. in wireshark  we see the process of ip distribution with the help of dora process 
dora means = d stands for  = discovery    o stands for offer    r stands for request and   a stands for acknowledgement
 # we give subnet in the form of bits like 
 ex- 123.120.34.4  subnet is 255.0.0.0
 in form of bit we should give 8 bit
#in wireshark  server side pe 67 port used hota hai aaur client side pe 68
#static ip - hamesha ek jaisi 
#dynamic ip-change hoti rehti hai


 