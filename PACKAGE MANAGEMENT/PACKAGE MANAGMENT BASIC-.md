Package management: 

Package Managers are used to handle software installation, removal, and updates.  

Here are the two most widely used package managers: 

- RPM 
    
    - YUM , DNF 
        
- DPKG 
    
    - APT 
        

1. Red Hat Package Manager (RPM) 

- RPM is the default package manager for Red Hat-based distributions, such as RHEL (Red Hat Enterprise Linux), CentOS, Fedora, and others. 
    

- It is an open packaging format that can be used on multiple Linux and UNIX systems, not just Red Hat-based ones. 
    

- File Extension: RPM packages end with the . rpm extension. 
    

- Usage: 
    
    - RPM is used for installing, uninstalling, upgrading, querying, listing, and checking RPM packages on Linux systems. 
        
    - It can handle software packages, documentation, and source code. 
        

- Two Types of RPM Files: 
    

- RPM (Binary RPM): Contains the compiled version of the application or libraries. 
    
- SRPM (Source RPM): Contains the source code for the application. These are typically used for compiling the application from source on your system. 
    

- Common commands : 
    

|   |   |
|---|---|
|Install a package:|#rpm -i package. Rpm|
|Remove a package:|#rpm -e package-name|
|Query package:|#rpm -q package-name|
|Verify installed package:|#rpm -V package-name|
|List all installed packages:|#rpm -qa|

- Architecture Types: 
    
    i386: For 32-bit systems. 
    
    x86_64: For 64-bit systems. 
    
    noarch: For platform-independent software, i.e. , it can run on any architecture. 
    

- Contents of RPM Packages: 
    
    - The application or libraries to be installed. 
        
    - Documentation, such as man pages and other manuals. 
        
    - Configuration files associated with the application. 
        
    - SPEC File: A file with a . spec extension, which provides the instructions for creating the RPM package. It contains metadata about the package, such as its version, dependencies, and installation scripts. 
        

- RPM Database: 
    
    - The installation status of RPM packages is tracked in the RPM database, which is stored in the directory /var/lib/rpm  
        
    - This database contains information about the packages installed on the system, their version, and other metadata. 
        

- Packages name format : 
    
    - Filenames include the package name, version, release, architecture, and sometimes the distribution version. 
        
    - Example: 
        
        nginx-1.20.O-1.et7.ngx.x86_64. rpm (for Nginx in CentOS) 
        
        httpd-2.4.6-93.et7.centos.x86 64. rpm (for the Apache web server in CentOS) 
        
        - httpd: 
            
             This is the name of the package. In this case, it refers to Apache HTTP Server, a popular web server software. 
            
        - 2.4.6: 
            
             This represents the version of the package. Here, it indicates that this is version 2.4.6 of the Apache HTTP Server. 
            
        - 93:  
            
            This is the release number of the package. The release number indicates how many times the package has been repackaged or modified for the given version. In this case, it is the 93rd build or release for version 2.4.6. 
            
        - et7: 
            
             This part indicates the distribution and version for which the package is built. et stands for Enterprise Linux,which refers to RHEL (Red Hat Enterprise Linux) and its derivatives (tike CentOS, Oracle Linux, etc.). 7 indicates the version of the operating system. So, e17 refers to a package built for RHEL/CentOS 7. 
            
        - centos: 
            
            This part indicates that the package is specifically built for the CentOS distribution, which is a community-driven project based on RHEL. 
            
        - x86_64: 
            
             This indicates the architecture of the package. x86_64 means the package is built for a 64-bit architecture. This is the standard architecture for modern systems. 
            
        - . rpm:  
            
            This is the file extension that indicates the file is an RPM package. The . rpm format is used by RPM-based Linux distributions for packaging software .1 
            
        

- YUM/DNF: These are front-end toots built on top of RPM for easier package management, especially for resolving dependencies . 
    

- Install a package: sudo yum install package-name or dnf install package-name 
    
- Update all packages: sudo yum update or sudo dnf update 
    

2. Debian Package Manager (dbkd) 

- dpkg is the default package manager for Debian-based distributions, such as Debian itself, Ubuntu, and others. 
    

- Fite format: " . deb" 
    

Common commands: 

|   |   |
|---|---|
|Install a package:|#dpkgl -i package.deb|
|Remove a package:|#dpkg -r package-name|
|List installed packages:|#dpkg -l|
|Query package:|#dpkg -s package-name|
|Fix broken dependencies:|#apt-get install -f|

|   |   |
|---|---|
|Debian based OS : which has dpkg|blackbox , arch , parot , kali, debian , ubuntu, etc|

- APT :  APT (Advanced Package Toot) is a higher-level package management tool for Debian-based systems, simplifying the installation, upgrading, and removal of packages. 
    

- Install a package: sudo apt install package-name 
    
- Update package list: sudo apt update 
    
- Upgrade installed packages: sudo apt upgrade