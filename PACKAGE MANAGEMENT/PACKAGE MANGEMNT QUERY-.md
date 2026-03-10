debian me bhi rpm chala saakte aur rhel me dpkg chala sakte hai jab jiski jarurat ho
cat /etc/os-release me jake os info le sakte hai
uname -a sse bhi le sakte hai
/var/lib/rpm me jake dekh sakte hai konse konse package installl ha 
# rpm commands=
[rpm --version] for version
[rpm --help] for help
#query - 
1.  [rpm -q] then package name - ki ye package install hai ki nhi                                 ab isme dikkat hai ki har packagee ka pura nam dena padega isliye hum
2. [rpm -qa ] - har package jo install hai usme dhundega
3. total package kitne install hai  [rpm -qa | wc -l]
4. [rpm -qa --last] abhi kuch install kara hai wo aayega
5. [rpm -qa | grep jo packet chaiye wo]
6. [rpm -qi ] for information
7. openssh vim googlechrome packageme dekh lo
8. install package me .rpm nhi likha aata
9. [rpm -ql package name] uske sath konse konse paccklages aai aur kha place hai
10. ab us package ki configuration file chaiye toh-
11. [rpm -qc package name] toh uski configuration file aa jayegi
12. [rpm -qd package name] toh uski documentation file aa jayegi
13. [rpm -qR package name] toh uski dependency aa jayegi
14. [rpm -qL package name] licencse pakage ka
15. [rpm -q --provides package name] provider aur vendore kon hai package ka
16. [rpm -q --dump package name] full package file detail
17. [rpm -qs package name] files me koi error toh nhi
18. [rpm -qf then file name] toh wo file kis package ke sath aai hai wo bataiga 
19.   like paaswd , shadow,gshadow,etc
20. ab nginix latest version dowload karke wget karke server me install kar lo
21. install karne se pehle wo package ki query karna ho toh 
22. [rpm -qip then jo install karna ho wo] i for info p for package q for query
23. [-V] verify packages
24. [nodeps]- dependencies mat bato
25. 