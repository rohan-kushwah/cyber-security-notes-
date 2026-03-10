# theory portion-
two types of hardrive or disks in linux-
sata- serial advanced technology attachment
ide-integrated drive electronics
two parts of ide-
ata-advanced technoloogy attachment
pata-parallel advanced technology attachment
![[Pasted image 20250127200401.png]]
ide has less storage capacity and there speed is very slow
sata ke pass jyada speed hoti
then ab ssd hdd aa gai
ide has 39 to 40 pins
![[Pasted image 20250127201013.png]]
hd detect hua toh wo ide disk hogi 
then master ke andar partition and slaave ke andar bhi partation
![[Pasted image 20250127201306.png]]
sd aa ra toh wo sata  disk hogi
has no primary secondry disk
sda-  port1
sdb -port2
sdc-port3
sdd-port4
then unke phir partation
#types of partition-
1.  primary-directly accessible partation of hardrive that can store os 
     1.1  - you can make 4 primary partition on mbr and 128 on gpt
2. extended-container of logical partation created when more that four partation is needed after the use of four primary partation

    2.0 - only one extended  partation is make
    2.1 logical-commonly use for organizing data or installing multiple os in single disk 
# practical portion-
in centos nine add 5 sata disk with diff storage vdi add
then start
hardrive jo lagate hai wo dev me dikhti
hardrives block device me aati
jo hardrives lagai hai unhe use karni hai toh-
1. partation banana padenge
2. partation ko format karna padenge
3. then partation ko mount karna padega
4. then ready to use
lsblk for list of block devices
harddrive lake dega aur uske partation bhi
blkid - jo partation hai unki info lake dega with unique id and partation id
fdisk-l- partation ki info dega
ek disk ka sata dekhan hai toh fdisk -l location de do
h.w - difference between mbr and gpt

The primary difference between MBR (Master Boot Record) and GPT (GUID Partition Table) is how they manage disk partitions and booting.

  1. **Partition Limits:**
    
    - **MBR:** Can support up to 4 primary partitions (or 3 primary partitions + 1 extended partition which can then have multiple logical partitions).
    - **GPT:** Supports up to 128 partitions by default on Windows (more on other systems), without needing extended partitions.
   2.  **Disk Size:**
    
    - **MBR:** Can only handle disks up to 2TB in size.
    - **GPT:** Can handle disks larger than 2TB (up to 9.4 zettabytes, which is far beyond current hardware capabilities).
  3. **Booting:**
    
    - **MBR:** Uses a bootloader in the first sector of the disk to start the operating system. It's tied to BIOS-based systems.
    - **GPT:** Uses a more modern boot method that works with UEFI (Unified Extensible Firmware Interface) systems. UEFI can provide better security features like Secure Boot.
 4.  **Reliability:**
    
    - **MBR:** Stores the partition information in one place, which can be a risk for data loss if the MBR becomes corrupted.
    - **GPT:** Stores multiple copies of the partition table across the disk, making it more resilient to corruption.
  5. **Compatibility:**
    
    - **MBR:** Older and supported by most systems, including legacy BIOS-based systems.
    - **GPT:** Newer and requires UEFI firmware, so it might not be compatible with very old systems or BIOS-only setups.
