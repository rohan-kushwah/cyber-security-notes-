1. pehle user banao then custom groups aur un users ko custom group me add kar lo ab sab me alg alg permission lagane ki dikkat nhi
#file system-
there are two types  of file system in windows =
1. fat-file allocation table
The **FAT file system** (File Allocation Table) is a type of file system used for organizing and managing files on storage devices like hard drives, USB flash drives, memory cards, and other types of storage media. It was one of the earliest file systems used in personal computers and remains in use today due to its simplicity and wide compatibility with various devices.
 fat purane devices me aata tha
1.1 no quota
1.2 no security= jha ghusedo chalu ho jayegi
1.3 supports max 4gb file in their storage
1.4 file ki bhi limits rehti hai
win 7 me jake pehle use iso dalte hai wha se space allocated karo then computer management me jake disk management me jake partition banao dono ntfs aur fat ka

2. NTFS-new technology file system 
TFS (New Technology File System) permissions ==control access to files and folders on Windows drives==. NTFS permissions are applied to all files and folders on a volume formatted with the NTFS file system. They are effective whether a file or folder is accessed locally or remotel
2.1 has quata
2.2 has security settings
2.3 supports 16tb aur 16 eb single file in their storage 
latest os me jake  pehle use iso dalte hai wha se space allocated karo then computer management me jake disk management me jake partition banao dono ntfs 
then usme file dal ke dekh sakte ho through share  folder  
#kisi bhi file ko permission allocate kari ho toh right click properties  then security advanced
1. has acl [access control list] kon kon us folder ko access kar saakta hai
2. has ownership jisne wo folder banaya administrator ownership change  kar sakta hai aur khud ko member bana ke hata bhi sakta hai 
3. owner us file me data rakhega toh sabko dikhega
4. then hum use permissions allocate karenge ki owner hi dekh pai koi aur nhi whi se  add aur enable inheritance karke
5. baki ko hata denge
Here are some basic NTFS permission levels:

- **Full Control**
    
    Users can add, change, move, and delete files and directories, and change permissions for them
    

- **Modify**
    
    Users can view and change files and their properties, but can't grant permissions to other users
    

- **Read and Execute**
    
    Users can read files and run executables, but can't change files and their properties
    

- **Read**
    
    Users can read files, their properties, and directories, but can't do anything else
    

- **Write**
    
    Users can write to a file, add files to a directory, and read files, but can't do anything else 
    

NTFS permissions can be set to "Allow" or "Deny". There are also advanced NTFS permissions, which are more granular than the basic levels. 

NTFS permissions differ from share permissions, which only apply to shared folders when accessed across a network. 

To see what permissions you're extending when sharing a file or folder, you can:

- Right click on the file or folder

- Go to Properties

- Click on the Security tab