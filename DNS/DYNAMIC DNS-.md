#first dhcp reconfigure with dns
1.1give both dns
1.2 give ips to client by dhcp 
1.3 make records of ips in server
problem har bar ip change hoga toh dns reach nhi kar payega isliye hum dynamicd dns denge
ip changes due  to dynamic and reecords doesnot chnage
1.4 then a rcord delete'
1.5.zone pe right click properties dynic dns non secure secure or making dns when
1.6 or goes to dhcp scope ki properties in dns 
1.6.1 always dynamic uodate
1.6.2 dynamically update dns records
1.7 then client pc set full accudian
1.8 then properties ip v4 advanced used dns surfix last option
then dynamic dns on and make automatically records
#2  forwarder 
2.1when attacker  attack toh hum secondry server bahar rakhenge ki uspe aatack ho toh koi dikkat nhi secondry server querys ko primary pe forward karega
2.2 if you want to forward give ip in forwarder of secondry server
#3 condition forwarder jo condition or domain name ho use particular dns pe bhejega
# authoritive me 
slave aur master
# non authoritive me
 recursive and dynamic dns
 