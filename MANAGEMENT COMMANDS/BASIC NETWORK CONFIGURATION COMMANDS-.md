IFCONFIG/IP  

The ifconfig(interface configuration command is used to configure network interfaces. It is now deprecated in most Linux
distributions in favor of the ip command, but it is still available for legacy systems.

	Display Network Interfaces
		
		# ifconfig

		This command lists all active network interfaces, showing details such as IP addresses, MAC addresses, and network
		status.
	
	Displays all interfaces, including those that are inactive.

		# ifconfig -a

	View a Specific Interface, Displays details for the enp0s
		
		# ifconfig enp0s3

	Assign an IP Address, Assigns the IP address 192.168.1.40 to enp0s3.

		# ifconfig enp0s3 192.168.1.40

	Assigns the IP address 192.168.1.35 with a /24 subnet mask.

		# ifconfig enpOs3 192.168.1.35 netmask 255.255.255.0

	To restart the network 
doubt --

		# systemctl restart network-online.target

	Assigns the IP address 192.168.1.42 with a /20 subnet mask.
		
		# ifconfig enp0s3 192.168.1.42/20


	Enable or Disable an Interface,Disables

		# ifconfig enp0s3 down

	Enables (brings up) the enp0s3 interface.
		
		# ifconfig enpOs3 up

	Assign Multiple IP Addresses to One Interface
		
		Creates a virtual interface enpOs3:1 192.168.1.42
		
			# ifconfig enpOs3:1 192.168.1.42
		
		Creates another virtuat interface enp0s3:2 with 192. 
			
			# ifconfig enpOs3:2 192.168.1.43
		
		Disable and Enable network interfaces, Disables the enp6s3 interface.
			
			# ifdown enp0s3
		
		Enables the enpOs3 interface.

			# ifup enp0s3


Since ifconfig is deprecated in modern Linux distributions,it is recommended to use the ip command instead:
	
	Display Network Interfaces
		
		# ip addr show
	
	Assign an IP Address, Assigns the IP address 192.168.1.40 to enp0s3.
		
		# ip addr add 192.168.1.40/24 dev enpOs3

	Disables the enpØs3 interface.
	
		# ip link set enp0s3 down
	
	Enables the enpOs3 interface.

		# ip link set enp0s3 up
	

	Display the Routing table  
	
	
In the "nmtui" command on Linux, "route" refers to ==a network path or configuration setting that specifies how data should be directed to a specific destination network==, essentially defining which gateway to use to reach a particular network address; when using nmtui, you can access and modify these routes to manage your network connectivity within the graphical text
		
		# ip route

	Add a New Route. To manually add a route to a specific network:
		
		# ip route add 192.168.29.42 via 192.168.29.1 dev enp0s3

	Delete a Route
		
		# ip route del 192.168.29.42/24

	Set a Default Gateway
		
		# ip route add default via 192.168.29.1 dev enp0s3

	Show Routes for a Specific Interface
		
		# ip route show dev enp0s3

	Flush All Routes	
		
		# ip route flush table main



Network Diagnostics: Ping, traceroute, and tracepath Commands
	
	Network diagnostic tools like Ping, traceroute, and tracepath help test connectivity and analyze network paths.
		
		ping Command - The ping command sends ICMP Echo Request packets to a host to check connectivity.
			
			Sends continuous pings to Google's public DNS server

				# ping 8.8.8.8

			Sends only two packet
			
				# ping 8.8.8.8 -c 2 

			Sending Large Packet Size - • sends a 65,504 yte packet to 192.168.1.1. The default packet size is 56 bytes.
				
				# ping 192.168.1.1 -s 65500

			Define a particular interface

				# ping -I enpOs3 192.168.1.1 -s 65500 -c 4 

			Sends 2 packets from enp0s3 with 65,500-byte size, using a custom payload (42 in hexadecimal) .
				
				# ping 192.168.1.1 -c 2 -s 65500 -I enpOs3 -p 42

			Interval and Payload - Sends 2 packets at 10-second intervals.
				
				# ping 192.168.1.1 -i 10 -c 2

			Adds a hexadecimal payload (42) .
				
				# ping 192.168.1.1 -i 10 -c 2 -p 42
			
			Resolves and pings armourinfosec com.
				
				# ping armourinfosec.com

			Sends packets as fast as possible.
				
				# ping 192.168.1.1 -f -s 65500



Traceroute maps the path packets take to reach a destination./


The traceroute command in Linux is ==a command-line tool that shows the path of network packets from the source to a destination==. It's a common tool for diagnosing network issues.


	Installation:
		
		# yum install traceroute               # RHEL/CentOS
		
		#apt install traceroute               Debian/Ubuntu

	
	Shows the route packets take to reach Google's DNS.
	
		# traceroute 8.8.8.8

	Traces the route to armourinfosec.io.
		
		# traceroute armourinfosec.io

	TCP Traceroute, Uses TCP instead of ICMP for tracing (useful when ICMP is blocked) .
		
		# tcptraceroute 8.8.4.4


Tracepath Command

The tracepath command in Linux is ==used to identify network issues like packet loss, high latency, or MTU issues==. It uses the Path MTU Discovery (PMTUD) mechanism to determine the maximum transmission unit (MTU) along a path


Installation
	
	# yum install iputils iputils-tracepath 		# RHEL/CentOS
	
	# apt install iputils-tracepath					# Debian/Ubuntu


	Traces the path to 8.8.8.8
		
		# tracepath 8.8.8.8




-i = interval 
-c = packet count 
-p = payload
-s = packet size
-I = Interface






# cd /etc/sysconfig/network-script
# vim /etc/sysconfig/network-scripts/ifcfg-enp0s3