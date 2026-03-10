sda a ko mat chedna nhi toh linux gaya
then jha partation create karne hai usko
#fdisk location of disk toh wo disk ke andar le jayega
 #concept-ek disk hai usko pehle hame gpt lagana hai ya mbr boot ke liye
then ab jab partation bante hai toh usme fille type dete ki linux hai swap hai or others ab jaise linux hai toh jabbhi boot hoga bios kernel ko linux file me dhundega then ab jab hu un partation ko format karte hai mkfs se uska matlab un partation me jo data rakhenge unke liye konsa file system hoga 
![[WhatsApp Image 2025-01-29 at 19.27.06_16b03bad.jpg]]
# creating partation- switches

    m- for help
     #directly partation toh mbr me hi banega but gpt karna hai toh gdisk gdisk or use fdisk me g
    n- new partation
    p- primary partation
    e-for extended partation
    then partation number dega mbr hai toh 4 gpt hai toh 128 
    extended ek hi bana sakte ho
    logical kitte bhi bana sakte ho 5  se chalu honge
    then storage sector
     max size - me jo apko dena ho like +25G
    p- partation table print
    d-delete the partation
    by default last hataiga
    space remaining ho toh bhi 4 se jyada primary nhi bana sakte mbr me
    yadi mene extended hataya toh bakki logical bhi hataiga 
    q- se bhar without changes 
    w- se save and exit
    t- file system aur partation id change karni ho toh 
    l- se list aaigi file systems ki
    ek ntfs ek fat do linux le lo
# format of partation-
     file system dalna hai partation pe-
     mkfs - konse konse file system se format kar sakte hai
     like- mkfs.ext location of partation
     partation format ho jaye tab uuid aajti blkid me
     ntfs chiye toh yum install ntfs*
# mounting of partation-
     df -h se dikh jayega kon mount hai kon nhi
     mnt ke andar mount kar do 
     mount -t file system type partation location
     umount se unmount kar do
     umount location of partation
     ye temporary mnt hai reboot karte hi hat jayeg 
     PARTATION ID CHANGGE HO JAYEGA AFTER REBOOT
     PARTPROBE SE WO MNT KO REFRESH KAR DETA HAI
# READY FOR USE-
![[WhatsApp Image 2025-01-28 at 18.02.00_fabae649.jpg]]
![[WhatsApp Image 2025-01-28 at 18.02.00_475f7606.jpg]]
![[WhatsApp Image 2025-01-28 at 18.01.54_f66491c1 1.jpg]]
