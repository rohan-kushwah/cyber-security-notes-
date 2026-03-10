#link and cut paste commands 
#pehle ek jagah se particular file ko copy kare ek particular extension ke sath
*.log 
#aur ek pure extension wali like=* * .*
example command 
ex= cp -v /var/log/* .
toh isme pura folder layega 
ex=cp -v /var/log/* ~/desktop/ d4/
isme sirf files layega 
#double .. ek directory piche past
# mv cut paste command
like move from one place to another
same place pe mv karenge toh wo rename ho jayega
by dfalault recursive
then use acc to your use
use wildcard and other things
# ln link command
# hard link
jha do file links ho same number of files
		no of files check=ls l wc -l 
		link banate hai shortcut ke liye
		#how to make links aur shortcuts
		1.ex ln /var/log/messages my_hard_links yadi current pe ho toh nhi toh pura nam
	
		ln command source and types of links
		#hard links always stay when the original file deleted
		hard links take same sp[a]ce like original file
# soft link
ex=ln  -s /var/log/messages my_soft_links
when original folder delete it also deleted
