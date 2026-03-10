nmcli connection modify enp0s3 ipv4.addresses 192.168.1.26/24
nmcli connection modify enp0s3 ipv4.gateway 192.168.1.1
nmcli connection modify enp0s3 ipv4.dns 8.8.8.8
nmcli connection modify enp0s3 ipv4.method manual


nmcli connection modify enp0s3 ipv6.method "disabled"

nmcli connection up enp0s3 
also nmtui
