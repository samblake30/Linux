# _Configure the System RAID_

## _Prequisite:-_
 *  _Centos 7 Machine_
 * _mdadm package installed_
 * _Two Data Volumes attached  to machine for performing the RAID_
 
## _Steps to perform:-_
 * _First check if the attached disks are available_
<p align="centre">
  <img width="950" height="150" src="https://github.com/samblake30/Linux/blob/main/src/img1.png">  
</p>

### ***Note:*** _Here we can create partition using both inbuilt utilities ![fdisk](https://img.shields.io/badge/Utility-fdisk-yellow?style=plastic&logo=appveyor) and ![parted](https://img.shields.io/badge/Utility-Parted-orange?style=plastic&logo=appveyor) but, for demo I have already taken backup of the partition tables created manually using the above mentioned utilities and here with the help of the ![sfdisk](https://img.shields.io/badge/Utility-sfdisk-brightgreen?style=plastic&logo=appveyor) we will restore the partition table with correct label and sizes._

### _Partition Table files:-_

Device Name      |  Partition Table Info
-----------      | --------------------
:point_right: _/dev/nvme1n1_  | ***![nvme1n1.txt](https://github.com/samblake30/Linux/blob/main/src/Partition%20files/nvme1n1.txt)***
:point_right: _/dev/nvme2n1_  | ***![nvme2n1.txt](https://github.com/samblake30/Linux/blob/main/src/Partition%20files/nvme2n1.txt)***


 * _Create 3 partitions in /dev/nvme1n1 device_
   ```bash
   sfdisk /dev/nvme1n1 < nvme1n1.txt
   ```
   
   ```bash
   [root@b1e95f64d31c ~]# lsblk /dev/nvme1n1
   NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   nvme1n1     259:0    0    2G  0 disk
   ├─nvme1n1p1 259:4    0  200M  0 part
   ├─nvme1n1p2 259:5    0  200M  0 part
   └─nvme1n1p3 259:6    0  200M  0 part
   ```
* _Create 7 partitions in /dev/nvme2n1 with 3 as primary 1 as extended and 3 as logical_

   ```bash
   sfdisk /dev/nvme1n1 < nvme1n1.txt
   ```
   
   ```bash
  [root@b1e95f64d31c ~]# parted -l /dev/nvme2n1
  Model: NVMe Device (nvme)
  Disk /dev/nvme0n1: 21.5GB
  Sector size (logical/physical): 512B/512B
  Partition Table: msdos
  Disk Flags: 

  Number  Start   End     Size    Type     File system  Flags 
  1      1049kB  21.5GB  21.5GB  primary  xfs          boot


  Model: NVMe Device (nvme)
  Disk /dev/nvme1n1: 2147MB
  Sector size (logical/physical): 512B/512B 
  Partition Table: msdos
  Disk Flags: 

  Number  Start   End    Size   Type     File system  Flags
  1      1049kB  211MB  210MB  primary               raid
  2      211MB   420MB  210MB  primary               raid
  3      420MB   630MB  210MB  primary               raid


  Model: NVMe Device (nvme)
  Disk /dev/nvme2n1: 2147MB
  Sector size (logical/physical): 512B/512B
  Partition Table: msdos
  Disk Flags: 

  Number  Start   End     Size    Type      File system  Flags
  1      1049kB  200MB   199MB   primary                raid
  2      201MB   400MB   198MB   primary                raid
  3      401MB   600MB   199MB   primary                raid
  4      601MB   2100MB  1499MB  extended               lba
  5      602MB   800MB   198MB   logical                raid
  6      801MB   1000MB  199MB   logical                raid
  7      1001MB  1200MB  198MB   logical                raid
  ```

 * _Check for the mdadm utility is installed_
    ```bash
    rpm -q mdadm
    ```
    * _If not installed then run below command_ :point_down:
    ```bash
    yum install -y mdadm
    ```
