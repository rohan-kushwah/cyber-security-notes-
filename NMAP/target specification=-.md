# Target Specification in Nmap

Nmap provides versatile options for specifying target hosts or networks to scan, allowing you to define single hosts, multiple hosts, ranges, subnets, and use input files to automate scans. Below is an organized overview with examples showing how to specify targets effectively.

## Single and Multiple Hosts

Scan one or multiple hosts/domains by listing them separated by spaces:

```shell
nmap armourinfosec.com thehackersworld.com
```

```shell
nmap 192.168.1.35 192.168.1.1
```

```shell
nmap -v 192.168.1.31 192.168.1.1 192.168.1.247
```

## Using IP Ranges and Subnets

Scan all hosts in a subnet by using CIDR notation:

```shell
nmap 192.168.1.0/24
```

```shell
nmap 192.168.56.1/24
```

Scan IP ranges with hyphen-separated numbers:
```shell
nmap 192.168.1.1,12,13,14,235
```
```shell
nmap 192.168.1.1-254
```
```shell
nmap 192.168.1.1-51,250-254
```
```shell
nmap -v 192.168.1.1-51,250-254
```
```shell
nmap 192.168.1.1-10,50-60,100,200,240-254
```
```shell
nmap -v 192.168.1.2.1-100
```
```shell
nmap 192.168.1-3.1/24
```
```shell
nmap 192.168.1,2.1/24
```
Scan multiple specific IPs separated by commas:
```shell
nmap 192.168.1.1,5,27,100,207
```
```shell
nmap 192.168.1.1,10,50,60,100,200,244,231,31,61
```
Scan a restricted range within subnet:
```shell
nmap 192.168.1.200-254
```

## Using Input File for Target List

Create a text file (e.g., ip.txt) containing target IPs or hostnames:
```shell
vim ip.txt
```

Contents of `ip.txt`
```
192.168.1.1
192.168.1.10
192.168.1.31
192.168.1.61
192.168.1.100
192.168.1.101
192.168.1.102
192.168.1.103
192.168.1.104
```

Scan the list of targets using the -iL option:
```shell
nmap -v -iL ip.txt
```

## Random Hosts Scanning

Scan a specified number of random IP addresses on the Internet:
Scans 10 random hosts

```shell
nmap -v -iR 10
```

## Excluding Hosts from Scans

Exclude specific IPs or ranges from a subnet scan using `--exclude`:

```shell
nmap -v 192.168.1.1/24 --exclude 192.168.1.1-10
```

```shell
nmap -v 192.168.1.1/24 --exclude 192.168.1.1-25
```

```shell
nmap -v 192.168.1.1/24 --excludefile ip.txt
```

```shell
nmap -v 192.168.1.1/24 --exclude 192.168.1.52-100,250
```

## Utility for Generating IP Lists

Install `prips` to generate IP ranges easily:

```shell
apt install prips
```

Use `prips` to list all IPs in a subnet or range:

```shell
prips 192.168.1.0/24
```

```shell
prips 192.168.1.0/28
```

```shell
prips 192.168.1.0/26 | grep 192.168.1.231
```

## Summary

Nmap's target specification capabilities offer a wide range of flexible options from scanning individual IPs, multiple hosts, entire subnets, to using input files and excluding hosts dynamically. This flexibility allows for efficient and precise scanning tailored to varying network sizes and conditions.

Use these target specifications in your scan commands to define exactly which hosts or networks you want to analyze.

---
---