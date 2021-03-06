#### 41. Filesystem Analysis: EXT Filesystem Basics

###### Extended Filesystem

- Based on the Unix File System (```UFS```) 
- Simpler than UFS
- Meant to be robust with good performance 
- Most common filesystem
- ```Ext2``` is common for ```partitions that don't change much```
- ```Ext3/4``` are ```journaling``` and used most often

###### Ext2 Structure

![Image of ext2](images/41/1.jpeg)

###### Ext2 Inodes

![Image of ext2](images/41/2.jpeg)

###### Ext Optional Features

- ```Compatible``` – if OS doesn't support feature it can still safely mount filesystem
- ```Incompatible``` – if OS doesn't support feature the filesystem should not be mounted
- ```Compatible Read Only``` – if OS doesn't support feature the filesystem can be mounted but not for writing
- Suspected attacker might have non-standard extensions

###### The Superblock
- ```1024 bytes``` from start and ```1024 bytes``` long
- Repeated in first block of each group
- The superblock contains	- Block size
	- Total blocks
	- Blocks per block group
	- Reserved blocks before the first block group 
	- Total inodes
	- Inodes per block group
	- Volume name
	- Last write time
	- Last mount time
	- Path where the file system was last mounted 
	- Filesystem status (clean?)

###### [```Setup```](http://imagemounter.readthedocs.io/en/latest/installation.html)
```sh
$ sudo apt-get install python-setuptools xmount ewf-tools afflib-tools sleuthkit
$ pip install imagemounter
$ imount --check
```

###### ```fdisk``` - manipulate disk partition table

```sh
u64@u64-VirtualBox:~/Desktop$ sudo fdisk 2015-3-9.img

Welcome to fdisk (util-linux 2.27.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): p
Disk 2015-3-9.img: 18 GiB, 19327352832 bytes, 37748736 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x0004565b

Device        Boot    Start      End  Sectors Size Id Type
2015-3-9.img1 *        2048 33554431 33552384  16G 83 Linux
2015-3-9.img2      33556478 37746687  4190210   2G  5 Extended
2015-3-9.img5      33556480 37746687  4190208   2G 82 Linux swap / Solaris

Command (m for help): q

u64@u64-VirtualBox:~/Desktop$
```

###### ```fsstat``` - Display general details of a file system

```sh
u64@u64-VirtualBox:~/Desktop$ fsstat -o 2048 2015-3-9.img
FILE SYSTEM INFORMATION
--------------------------------------------
File System Type: Ext4
Volume Name:
Volume ID: 6d3257cce44c5499d141d359183193c1

Last Written at: 2015-03-12 16:30:28 (PDT)
Last Checked at: 2015-03-05 19:37:57 (PST)

Last Mounted at: 2015-03-12 16:29:39 (PDT)
Unmounted properly
Last mounted on: /

Source OS: Linux
Dynamic Structure
Compat Features: Journal, Ext Attributes, Resize Inode, Dir Index
InCompat Features: Filetype, Extents, Flexible Block Groups,
Read Only Compat Features: Sparse Super, Large File, Huge File, Extra Inode Size

Journal ID: 00
Journal Inode: 8

METADATA INFORMATION
--------------------------------------------
Inode Range: 1 - 1048577
Root Directory: 2
Free Inodes: 863973
Inode Size: 256

CONTENT INFORMATION
--------------------------------------------
Block Groups Per Flex Group: 16
Block Range: 0 - 4194047
Block Size: 4096
Free Blocks: 3042264

BLOCK GROUP INFORMATION
--------------------------------------------
Number of Block Groups: 128
Inodes per group: 8192
Blocks per group: 32768

Group: 0:
  Block Group Flags: [INODE_ZEROED]
  Inode Range: 1 - 8192
  Block Range: 0 - 32767
  Layout:
    Super Block: 0 - 0
    Group Descriptor Table: 1 - 1
    Group Descriptor Growth Blocks: 2 - 1024
    Data bitmap: 1025 - 1025
    Inode bitmap: 1041 - 1041
    Inode Table: 1057 - 1568
    Data Blocks: 9249 - 32767
  Free Inodes: 668 (8%)
  Free Blocks: 22866 (69%)
  Total Directories: 370
  Stored Checksum: 0xB0A2

Group: 1:
  Block Group Flags: [INODE_ZEROED]
  Inode Range: 8193 - 16384
  Block Range: 32768 - 65535
  Layout:
    Super Block: 32768 - 32768
    Group Descriptor Table: 32769 - 32769
    Group Descriptor Growth Blocks: 32770 - 33792
    Data bitmap: 1026 - 1026
    Inode bitmap: 1042 - 1042
    Inode Table: 1569 - 2080
    Data Blocks: 33793 - 65535
  Free Inodes: 7521 (91%)
  Free Blocks: 1434 (4%)
  Total Directories: 2
  Stored Checksum: 0x328A

Group: 2:
  Block Group Flags: [INODE_UNINIT, INODE_ZEROED]
  Inode Range: 16385 - 24576
  Block Range: 65536 - 98303
  Layout:
    Data bitmap: 1027 - 1027
    Inode bitmap: 1043 - 1043
    Inode Table: 2081 - 2592
    Data Blocks: 65536 - 98303
  Free Inodes: 8192 (100%)
  Free Blocks: 2119 (6%)
  Total Directories: 0
  Stored Checksum: 0xE8A0
<---snip--->
Group: 126:
  Block Group Flags: [INODE_UNINIT, BLOCK_UNINIT, INODE_ZEROED]
  Inode Range: 1032193 - 1040384
  Block Range: 4128768 - 4161535
  Layout:
    Data bitmap: 3670030 - 3670030
    Inode bitmap: 3670046 - 3670046
    Inode Table: 3677216 - 3677727
    Data Blocks: 4128768 - 4161535
  Free Inodes: 8192 (100%)
  Free Blocks: 32768 (100%)
  Total Directories: 0
  Stored Checksum: 0x3574

Group: 127:
  Block Group Flags: [INODE_UNINIT, INODE_ZEROED]
  Inode Range: 1040385 - 1048576
  Block Range: 4161536 - 4194047
  Layout:
    Data bitmap: 3670031 - 3670031
    Inode bitmap: 3670047 - 3670047
    Inode Table: 3677728 - 3678239
    Data Blocks: 4161536 - 4194047
  Free Inodes: 8192 (100%)
  Free Blocks: 32512 (100%)
  Total Directories: 0
  Stored Checksum: 0x9227
u64@u64-VirtualBox:~/Desktop$
```