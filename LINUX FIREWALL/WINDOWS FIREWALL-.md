there profile of windows firewall-alg alg jagah jate tog alg alg profile apply ho jati profiles are the set of rules
1. private 
2. public
3. domain- activate when pc are domin join
in advanced inbound or outbound traffic-
Inbound traffic refers to any data coming into a system or network from an external source. This includes requests from remote devices, users, or servers trying to communicate with services running on the local machine. 

Outbound traffic refers to any data leaving a system or network and going to an external destination. This includes requests made by local applications, servers, or users to remote systems or websites.
example - ping is not working allow icmp echo

add iis all options-
then firewall ne iska port allow kar diyahoga
ab ek aur port ko bind karte iis se 
ab iis usko allow nhi karega hame manually karna padega\
jabhi wo us port par chalega


then program/service allow-
wo program default jo port pe run karega wo allow lar dega
go on task manager
example - iis ko as a program run karna ho ki wo default port 80 pe na chalkke sab pe chale toh 
pehle toh  iis program ki location =cdrive/windows32/inetsrv/w3wp.exe
inbound me add rule
program


cmd se bhi app ko run akr sakte hai location do bus
netcat  download karo unzip karo nc .exe.nc.64 windows mw dal do
then nc -nlvp port 
wo first time autoamtically rule banane ki try karega
nhi karoge toh wo block me patak dega
ab program ka rule dalenge toh sabse chalega
