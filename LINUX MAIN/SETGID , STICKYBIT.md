rwx dekh lo user aur group pe
Files

- Read (r): Allows viewing the contents of the file.
- Write (w): Allows modifying the contents of the file. [doubt permission dene ke bad bhi likh nhi pa ra  ]
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
# for files and binary-
setgid- group pe dete hai execute ke section me
koi bhi banda us binary ko run karega toh aisa lagega wo us binary ke group ka member hai jabki wo nhi hai  auur wo group ki power se run hogi
yadi koi uske group ka member hai toh wo by default uske pass power rahegi
permission wo file pe hogi group section me whi uske user ke pass jayegi toh wo uske hisab se changes kar payega
binary script pe karke dekh lo nano se dekh lo
# for directory-
then ab hr emp sells directory pe-
then ab group ko mene permission de di rwx
aur us directory ko mene uske nam ke group ko add kar diya
ab uske user changes kar skte hai hai us directory me dusre nhi 
ab jo me changes kar rha hu  toh jo file aur directory uske andar ban rhi hai usme group jo user bana raa uska aa ra ab muje us directory ka group hi uske andar ke data me chaiye toh]
aur jo permission wo directory pe ho wo  un group user se banai file pe bhi ho toh [ write ki nhi de ra ]
then hum [group pe special permission denge ]
[chmod g+s directory]
toh ab group user se banai gai directory pe bhi wo permission aur group apply hoga
![[Pasted image 20250114005210.png]]
numeric me permmisson dena ho toh 2775
2 lagega group ko special permission deni ho toh
# sticky bit -
ab ek folder me other users ko read write permissions hai 
toh wo usme changes kar ke file rakh saakta hai
but ab koi bhi user ake usko delete kar dega toh ye dikkat hai
iske liye sticky bit permission use hogi x ke section me t
ab jisne wo file us directory me banai hai whi use delete kar sakta hai
chmod o+t directory name
numeric 1777
1 se hogi sticky bit
![[Pasted image 20250114010823.png]]
