# _Configure the system RAID using mdadm_

## _Prequisite_
 *  _Centos 7 Machine_
 * _mdadm package installed_
 * _Two Data Volumes attached  to machine for performing the RAID_
 
### _Steps to perform_
 * _First check if the attached disks are available_
<p align="centre">
  <img width="950" height="150" src="https://github.com/samblake30/Linux/blob/main/images/img1.png">  
</p>

### ***Note:*** _Here we can create partition using both inbuilt utilities ![fdisk](https://img.shields.io/badge/Utility-fdisk-yellow) and ![parted](https://img.shields.io/badge/Utility-Parted-orange) but, for demo I have already taken backup of the partition tables created manually using the above mentioned utilities and here with the help of the ```sfdisk``` utility we will restore the partition table with correct label and sizes._

 * _Create 3 partitions in /dev/nvme1n1 device_
<p align="centre">
  <img width="950" height="150" src="https://github.com/samblake30/Linux/blob/main/images/img1.png">  
</p>
