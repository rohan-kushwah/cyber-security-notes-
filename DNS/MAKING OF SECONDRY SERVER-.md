#firstly make another server

1.there are multiple secondry server 
2.work in the absence of primaary server 
3.secondry server used for load blancing
4.secondry server have copy of zone data from primary server
5.there is no chnages of data in secondry server
# after install server 
1. ip configure
2. then configure name
3. then give to all hosts
4. zone name same
5. the install dns
6. check nslookup query
7. then go primary
8. make aa record of secondry server
9. then ping
10. then name resolve
11. then add name serve zone pe right click properties add ns
12. then give accudian
13. then allow for zone transfer
14. only name server on tab
15. go secondry make zone 
16 start of authority record 
1.serial no always increase when there is chnages in primary server
serial no use by secondry server to check new zone transfer
17.then go to client give secondry dns'
18.then both server wireshark
19.primary ka network disaable karoge jab hi wo secondry me requests bataiga
20.then zone query from client
21.axfr full zone transfer ixfr incremental zone transsfer uses tcp port
22.then wireshark me dono port or me 
23.secondry restart check in master
24.axfr kar ra ya ixfr
25.then reverse same method
