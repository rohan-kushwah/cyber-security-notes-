iso jab dalte hai toh wo by default ide me lega 
sata karna hai toh ide controller hata do
#error - jab hum mnt karte hai kisi folder me then data place karte hai toh after reboot and again mnt data usme nhi rehta kyunki usi id change ho jati hai iske liye use sahi se lne by line format kare
sda1 to sbb1 like that
ex- jaise device lgaya ab wo sata 1 pe hai
then ab unplug kiya aur waps lagay toh wo sata 2 pe lagega
toh ab wo mnt to dusri jagah ho jayega isliye data chala jata
# permanent mnt-
entry in /etc/fstab - jab bhi pc boot hota hai toh iske andar jo bhi rehta use mount kar deta hai
1. by uuid
2. by id ex - sdb1 sdb2
3. by label  ' 
![[Pasted image 20250129200529.png]]
first uuid -
then location jha mount karna hai
thenkonse file system me format karna hai
then permission
own boot- jab pc chaalu hoga toh partation ko check karna hai ki nhi 0 nhi 1 ha
ab konsi pehli check hogi uuska no
 xfs_admin -L data /dev/sde1
# by id-
	/dev/sda1/   /      xfs     default  0 0
	ab ye sda1 / ke andar permanent mount ho jayega  
df-h  se dekh lo mnt hue ki nhi

# by label-
#problem= jab hum disk ko idhar udhar karenge toh partation id bhi idhar udhar hogi toh jo mnt kiya hai partation ko fstab me wo toh change ho jayega use wo data us partation me milega hi nhi

#solution=by label mnt karenge toh ab partation id change bhi ho jaye toh dikkat nhi 
label matlab jaise hum kai bar c drive  a nam game rakh dete like that-
ab ext4 file system hai toh label ke liye command e2label
ab xfs ko label denaa ho toh xfs_admin -l then name location of partation
blkid se label dikh jayene
LABEL=NAME THEN same as id 
xfs_admin -L data /dev/sde1
mount -a se kuch galti ho fstab ki entry me toh bata detaa
# by uuid-
     uuid=of partation   location  file system    permissions 0 0
# extend of swap file system-
swap - virtual ram used after main memory full
jaise shuru me jo swap banaya tha usse badana ho toh use karte
free-h se swap dikh ajyega
then swapon command
then ab do swap banao kisi disk me
then mkswap se usnhe format kar lo
then ab swapon then partation name toh swap bad jayega
ab ye temporary hai permanent  rakhna hai toh fstab me entry kar do
by uuid of swap partation or file system swap rakhna location none
swap hatana ho toh swaooff location of partation
ab konsa partation pehle us karna ho toh 
swapon -p partation

