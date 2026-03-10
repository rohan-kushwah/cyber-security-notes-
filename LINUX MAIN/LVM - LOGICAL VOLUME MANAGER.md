Logical Volume Manager (LVM)

Logical Volume Manager (LVM) is a storage management solution in Linux that provides flexibility and advanced features for managing disk space. It allows dynamic resizing of partitions, snapshot creation, and easier storage expansion.
![[Pasted image 20250202021105.png]]

# 1. Key Components of LVM

- **Physical Volume (PV)** - The actual storage device (e.g., /dev/sdb, /dev/sdc).
- **Volume Group (VG)** - A pool of storage created from one or more PVs.
- **Logical Volume (LV)** - A partition-like structure inside a VG, where file systems are created.

# 2. Setting Up LVM

Step 1: Install LVM (If Not Installed)

Code

```
# apt install lvm2 -y  # Debian/Ubuntu# yum install lvm2 -y  # RHEL/CentOS
```

Step 2: Create Physical Volumes (PV)

- Identify available disks:

Code

```
# lsblk 
```

- Initialize the disk as an LVM physical volume:

Code

```
# pvcreate /dev/sdb /dev/sdc /dev/sdd /dev/sde /dev/sdf 
```
Verify:

Code

```
# pvs # pvdisplay 
```

Step 3: Create a Volume Group (VG)  
Create a volume group named vg_data:

Code

```
# vgcreate vg_data /dev/sdb /dev/sdc /dev/sdd /dev/sde 
```

Check VG details:

Code

```
# vgs # vgdisplay 
```

Step 4: Create a Logical Volume (LV)  
Create a 60GB logical volume named lv_storage:

Code

```
# lvcreate -L 60G -n lv_storage vg_data 
```
-L for storage and -n for name 
Or use 100% of the free space:

Code

```
# lvcreate -l 100%FREE -n lv_storage2 vg_data 
```
here -l to display
Verify:

Code

```
# lvs 
```
  . Formatting and Mounting the LVM Partition

Format the logical volume:

Code

```
# mkfs.ext4 /dev/vg_data/lv_storage# mkfs.xfs /dev/vg_data/lv_storage2 
```

Create a mount point and mount it:

Code

```
# mkdir /mnt/storage # mount /dev/vg_data/lv_storage /mnt/storage 
```

# 3. Resizing Logical Volumes

Extend an LV (Increase Size)

Code

```
# vgextend vg_data /dev/sdf 
```

4. Increase the size by 10GB:

Code

```
# lvextend -L +10G /dev/vg_data/lv_storage 
```

5. Resize the filesystem:

Code

```
# resize2fs /dev/vg_data/lv_storage 
```
 only for ext4 file system
6. Verify new size:

Code

```
# df -h /mnt/storage 
```
 # Shrink an LV (Reduce Size)

1. Unmount the volume:

Code

```
   # umount /mnt/storage
```

2. Check and reduce filesystem:

Code

```
   # e2fsck -f /dev/vg_data/lv_storage   # resize2fs /dev/vg_data/lv_storage 15G
```
e3fsck for ext4 and resize bhi ext4
1. Reduce LV size:

Code

```
   # lvreduce -L 15G /dev/vg_data/lv_storage
```

2. Remount the volume:

Code

```
   # mount /dev/vg_data/lv_storage /mnt/storage
```

for creating backup through snapshot-

# 4. **Create a snapshot of lv_storage:**

Code

```
    # lvcreate -L 5G -s -n lv_snapshot /dev/vg_data/lv_storage
```
snapshot /dev/vg_data/ ke andar snapshot namse aayega unknown language me 
like copy of partation data
jitti storage doge utti khayega ye pvs lvs aur vgs se dekh lo
jiska snapshot banaya hai usi ka file system snapshot ko diya jayeega
-s for snapshot create

1. **Mount the snapshot:**

Code

```
   # mount /dev/vg_data/lv_snapshot /mnt/snapshot 
```

2. **Remove snapshot after use:**

Code

```
   # umount /dev/vg_data/lv_snapshot    # lvremove /dev/vg_data/lv_snapshot 
```

# 5.Removing LVM Components

- Remove a Logical Volume

Code

```
# umount /mnt/storage# lvremove /dev/vg_data/lv_storage# lvdisplay
```

Remove a Volume Group

Code

```
# vgremove vg_data# vgdisplay
```

Remove a Physical Volume

Code

```
# pvremove /dev/sdb /dev/sdc /dev/sdd /dev/sde /dev/sdf # pvdisplay
```

Checking LVM Status

Code

```
Show all LVM partitions:# lvs Show volume groups:# vgs Show physical volumes:# pvs 
```
# some other commands-

pvdisplay
 pvdisplay  
 pvcreate /dev/sde1 
vgdisplay  
vgextend vg1 /dev/sde1  
lvdisplay  
lvextend /dev/vg1/lv2 --size 50G --resizefs  
lvextend /dev/vg1/lv2 --size 75G  
lvextend /dev/vg1/lv2 -L 50G  
lvreduce /dev/vg1/lv2 --size 25G --resizefs  
lvreduce /dev/vg1/lv1 --size 110G  
lvreduce /dev/vg1/lv2 -L 15G  
lvresize /dev/vg1/lv2 --size 90G --resizefs  
lvresize /dev/vg1/lv2 --size 40G --resizefs  
lvresize /dev/vg1/lv2 --size 75G  
lvresize /dev/vg1/lv2 --size 25G  
lvresize /dev/vg1/lv1 -L 50G  
lvresize /dev/vg1/lv1 -L 100G
#df -h  
xfs_growfs -n /dev/vgl/lv1  
xfs_growfs /dev/vgl/lv1  
xfs_info /dev/vgl/lv1  
 umount /d1 /d2  
 lvdisplay  
lvremove /dev/vg1/lv1  
lvremove /dev/vg1/lv2  
vgdisplay  
vgremove vg1  
 pvremove /dev/sdb1  
pvremove /dev/sdc1  
pvremove /dev/sdd1
# raid-
  mdadm --create /dev/md0 --level=0 --raid-devices=2 /dev/sdb1 /dev/sdc1
 mdadm --create /dev/md0 --level=striping --raid-devices=2 /dev/sdb1 /dev/sdc1
mdadm --create /dev/md1 --level=1 --raid-devices=2 /dev/sdd1 /dev/sde1 
mdadm --create /dev/md1 --level=mirror --raid-devices=2 /dev/sdd1 /dev/sde1