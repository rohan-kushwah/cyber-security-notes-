10 types of permission in linux 
1 additional hoti hai
ls -lh me right side jo dikhegi corner pe wo
![[Pasted image 20250107185414.png]]
# first  position me -                                                                            6.1 - wo subject 
normal file hai toh [-]
directory ho toh [-d]
link ho toh [-l] 
process file ho toh [-p]
block device ho toh [-b]
character device ho toh [-c] 

#process file - me jab linux run hota hai tab wo execute hogi
/proc me jayegi

#block file -Block device files in Linux are **special files that represent devices with block-level access**, such as hard drives, solid-state drives (SSDs), and other storage devices.
ls -l /dev | grep '^b'

#character file- Character device files are **associated with character or raw device access**. They are used for unbuffered data transfers to and from a device. Rather than transferring data in blocks the data is transfered character by character. One transfer can consist of multiple characters
ls -l /dev | grep '^c'
# 2 to 4 position - owner ke dwara banai jati hai
2 . me read 
3 . me write 
4 . me execute
yadi [-] aa ra toh no permission of read write execute 
• Read (r):
		○ Files: Allows viewing the contents of the file.
		○ Directories: Allows listing the contents of the directory.
	• Write (w):
		○ Files: Allows modifying the contents of the file.
		○ Directories: Allows adding, deleting, or modifying files within the directory.
	• Execute (x):
		○ Files: Allows running the file as a program/script.
		○ Directories: Allows accessing files and subdirectories within, and entering the directory.
		Here are the meanings of Read, Write, and Execute permissions for different types of files and devices:

Files

- Read (r): Allows viewing the contents of the file.
- Write (w): Allows modifying the contents of the file.
- Execute (x): Allows running the file as a program or script.

Directories

- Read (r): Allows listing the contents of the directory.
- Write (w): Allows adding, deleting, or modifying files within the directory.
- Execute (x): Allows accessing files and subdirectories within, and entering the directory.

Binaries

- Read (r): Allows reading the binary file.
- Write (w): Allows modifying the binary file.
- Execute (x): Allows running the binary file as a program.

Links

- Read (r): Allows reading the link.
- Write (w): Allows modifying the link.
- Execute (x): Allows following the link.

Block Devices

- Read (r): Allows reading data from the block device.
- Write (w): Allows writing data to the block device.
- Execute (x): Not applicable, as block devices are not executable.

Character Devices

- Read (r): Allows reading data from the character device.
- Write (w): Allows writing data to the character device.
- Execute (x): Not applicable, as character devices are not executable.
# 5 to 7 associated group ki permission
ye ls-lh me dikhega user ke bad uska primary associated group koi bhi user ko us group me jod doge toh same permission ho jayegi uske pass bhi
# 8 to 10 other user jo na group hai na owner hai
  1 . me read 
2 . me write 
3 . me execute
yadi [-] aa ra toh no permission of read write execute

# ls -lh info
first filed permissions
![[ls -lh.png]]
second field uske andar kha kha ja sakte hai directory not include files 
then owner of file
owner ka associated group
then space
then last modified
then file name
pehele se kisi bhi system directory me 2 folder aate hai[.] means current location and[..] current location se ek piche
# permission allocating-
#symbolic method - 
r -read
w-write'
x-execute
synatx-
user class = operation=permission
user class -
1. u -user owner
2. g- group
3. o-others
4. a -all other user groups etc 
operation (=,-,+)
[+]adding permissions
[-] removing of permissions
[=] do permission add karna ho toh 
permissions - r , w, x
example - chmod u+r file.txt
chmod o-w file1.txt
chmod g=rw file2.txt
#numeric method  
read = 4
write=2
execute =1
user class 
1. owner [u] ke liye 
2. group ke liye
3. others ke liye
  then jaise r+w+x ki permissions kisi user ko deni hai toh 
chmod  644 file.txt
ab owner ke pass r=w ki permission hai
group ke pass read ki
aur other user ke pass bhi read ki
11th permission
[.] hai toh no acl
[+] hai toh acl hai
