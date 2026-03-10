FIREWALL 

	A firewall is a security device that monitors and filters traffic between networks. It acts as a barrier between an organization’s internal network and the internet, protecting it from unauthorized access and malicious traffic.


# Inbound traffic-
in a firewall refers to the data packets that originate from external sources and are attempting to enter a network or a system. Essentially, it’s the incoming network traffic that the firewall must inspect and decide whether to allow or block, based on predefined rules.

For example:

- If someone on the internet tries to access a website hosted on your server, that request is considered *inbound traffic*.
- Similarly, if you have a remote access system (like a VPN), and someone from outside tries to connect to it, that's also inbound traffic.
- in inbound traffic we have deny list we should allow what kind of packets can enter

# outbound traffic-
Outbound traffic in a firewall refers to data packets that originate from within the network or system and are attempting to leave the network to an external destination. This type of traffic involves requests or data being sent from internal devices (such as computers, servers, etc.) to external systems or networks (like the internet).

For example:

- When you open a website in a browser, the request your device sends to access that site is considered *outbound traffic*.
- If an internal application sends data to a remote server for processing or storage, that would also be outbound traffic.
 in outbound traffic there is allow list for all usr we added deny for the traffic



	Firewall Setup 

		# rpm -qa | grep firewalld

		# rpm -qi firewalld

		If not installed the service 

			# yum install firewalld

			# apt install firewalld



	Firewalld Commands 

		# Systemctl status Firewalld 

		# Systemctl disable iptables

		# systemctl stop iptables(stops the Ip tables)


	Enable the firewalld at startup

		# systemctl enable firewalld

		# systemctl start firewalld

	Now View The status 

		# systemctl status firewalld 

	If its start then you cant view the IP at any address

			# systemctl stop firewalld

				# stops the service and the page can be loaded

	Restart Firewalld 

		# systemctl restart firewalld 

		# firewall-cmd --reload

	View the active Zones 

		# firewall-cmd --get-active-zones

	View the complete list 

		# firewall-cmd --list-all

Add posts to the firewalld allow list 

		# firewall-cmd --permanent --add-port=21/tcp   for ftp

		# firewall-cmd --permanent --add-port=80/tcp http

		# firewall-cmd --permanent --add-port=443/tcp   https

		# firewall-cmd --permanent --add-port=53/udp   dns

		# firewall-cmd --permanent --add-port=67/udp  dhcp 

		# firewall-cmd --permanent --add-port=110/udp    postoffice 

		# firewall-cmd --permanent --add-port=25/udp  smptp

To make all the settings load use:

		# firewall-cmd --reload

	View The refreshed Content 

		# firewall-cmd --list-all

	



# netcat tool=
Netcat (often abbreviated as **nc**) is a versatile networking tool that can be used in various ways to troubleshoot, test, and analyze network connections. When working with a firewall, Netcat can serve several useful purposes, such as:

### 1. **Testing Firewall Rules:**

Netcat can be used to test if certain ports are open or closed in the firewall. You can try to connect to a specific port to verify whether inbound or outbound traffic is being allowed or blocked by the firewall.

Example:

- To test if a remote server has port 80 (HTTP) open:
    
    bash
    
    Copy
    
    `nc -zv remote_server_ip 80`
    
    - `-z` means scanning for open ports.
    - `-v` is for verbose output, showing detailed information.
    - Netcat (often abbreviated as **nc**) is a versatile networking tool that can be used in various ways to troubleshoot, test, and analyze network connections. When working with a firewall, Netcat can serve several useful purposes, such 
    - Install netcat 

		# yum installl netcat 

    - 
		# apt install Netcat

	start the port at the server end 

		# nc -nlvp 21
		**`-n`**: This option tells Netcat to **avoid DNS resolution**
		**`-v`**: This stands for **verbose**
		**`-l`**: This flag tells Netcat to **listen** for incoming connections instead of initiating a connection.
		**`-p`**: This option specifies the **local port** that Netcat will use to listen for incoming connections or bind to.

	To listen or to connect on the Client use:

		# nc -v 192.168.29.21(server's IP) 21(port Number) 
		likho isme toh upar wale me dikheg'
		