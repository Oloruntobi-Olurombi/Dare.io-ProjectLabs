# Objective: Web Soltion With Wordpress

### LAUNCH AN EC2 INSTANCE THAT WILL SERVE AS "WEB SERVER".

#### Step 1 : Prepare a Web Server
- Launch an EC2 instance that will serve as "Web Server". Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.

![image](https://user-images.githubusercontent.com/40290711/128598774-3bd7b553-9f26-4b90-b5d6-e8d879e0121e.png)
- Attach all three volumes one by one the Web Server EC2 instance:
![Project62](https://user-images.githubusercontent.com/40290711/128598840-b982f345-05a1-4879-98a3-4088eb1b57c6.PNG)

- Open up the linux terminal to begin configuration
![project61](https://user-images.githubusercontent.com/40290711/128599072-a624d0b1-c208-4bb9-9246-3ba4196e7d01.PNG)

- Check if the three volumes are attached to the server via the command line using: 
lsblk 

![project63](https://user-images.githubusercontent.com/40290711/128598950-e4fc6bd9-b8de-437a-9e3d-cf29d1781dde.PNG)

- Use df -h command to see all mounts and free space on the server

![project64](https://user-images.githubusercontent.com/40290711/128599113-85b2be22-86a7-4522-8b77-ca13832fa505.PNG)

- Create a single partition on each of the 3 disks using:
sudo gdisk /dev/xvdf

![project65a](https://user-images.githubusercontent.com/40290711/128599227-baed3bd5-47dc-4acd-9f69-3cc7020798fb.PNG)

![Project65b](https://user-images.githubusercontent.com/40290711/128599252-664ee644-ab84-442e-9a3c-e2f4cdb38c9b.PNG)

sudo gdisk /dev/xvdg

![project66a](https://user-images.githubusercontent.com/40290711/128599275-bd09f157-1f70-4996-958b-88b02507ec90.PNG)

![project66b](https://user-images.githubusercontent.com/40290711/128599280-f2df89e0-8cbd-4898-b9e7-e32dace9cf67.PNG)

sudo gdisk /dev/xvdh
![project67a](https://user-images.githubusercontent.com/40290711/128599311-5b946dd3-dc4e-4628-9121-c0e58e43cdd4.PNG)

![project67b](https://user-images.githubusercontent.com/40290711/128599316-319d59a8-315c-45bd-a90b-d03fa458a922.PNG)

-  To view the newly configured partition on each of the 3 disks using:
lsblk

![project68](https://user-images.githubusercontent.com/40290711/128599383-6efe00fb-516f-47fe-9ff4-ecb55c9e638e.PNG)

- Install lvm2 package using:
sudo yum install lvm2

![project69](https://user-images.githubusercontent.com/40290711/128599419-bff5e6ed-49c4-4902-84f6-8cfdacbbed41.PNG)

- Check for available partitions running:
sudo lvmdiskscan

- Mark each of 3 disks as physical volumes (PVs) to be used by LVM using:
sudo pvcreate /dev/xvdf1 /dev/xvdg1 /dev/xvdh1

![project610](https://user-images.githubusercontent.com/40290711/128599538-4d252b7c-2d68-4ae0-88ab-1027b2d6c522.PNG)

- Verify that your Physical volume has been created successfully by running:
sudo pvs 

![project611](https://user-images.githubusercontent.com/40290711/128599573-15d5a1d2-4c9a-439f-a9ce-e8da2c613add.PNG)

- To add all 3 PVs to a volume group (VG) use:
sudo vgcreate vg-webdata /dev/xvdh1 /dev/xvdg1 /dev/xvdf1 

![project612](https://user-images.githubusercontent.com/40290711/128599607-42b16b24-e7e7-41c3-934d-7961c71a5506.PNG)

- Verify that the VG has been created successfully by running:
sudo vgs
![project612b](https://user-images.githubusercontent.com/40290711/128599659-4d9c24e7-8417-4f5e-a9ac-838f8fc6e4da.PNG)

- Create 2 logical volumes using:
sudo lvcreate -n app-lv -L 13G vg-webdata
sudo lvcreate -n logs-lv -L 14G vg-webdata

![project613](https://user-images.githubusercontent.com/40290711/128599762-0cbd53de-acb7-4e40-ad55-d76ece442a9b.PNG)

- Verify that the Logical Volume has been created successfully by running:
 sudo lvs
 
 - Verify the entire setup:
 sudo vgdisplay -v #view complete setup -VG, PV, and LV
 sudo lsblk
 
 - Format the logical volumes with ext4 filesystem using:
sudo mkfs -t ext4 /dev/vg-webdata/apps-lv

![project614](https://user-images.githubusercontent.com/40290711/128600031-249d2f38-788f-49fa-a23c-4647bb5074f2.PNG)


sudo mkfs -t ext4 /dev/vg-webdata/logs-lv
![project614b](https://user-images.githubusercontent.com/40290711/128600047-5b1ce063-dcf7-4080-b74d-e1703629d9f5.PNG)

- Create /var/www/html directory to store website files:
sudo mkdir -p /var/www/html
![project615](https://user-images.githubusercontent.com/40290711/128600137-8142d1fd-e2e0-4fc7-84a4-222cdcaebe6d.PNG)

- Create /home/recovery/logs to store backup of log data:
sudo mkdir -p /home/recovery/logs
![project616](https://user-images.githubusercontent.com/40290711/128600171-1e32c389-c685-4315-8bd3-9ded1674d317.PNG)

- Mount /var/www/html on apps-lv logical volume:
sudo mount /dev/webdata-vg/app-lv /var/www/html/

![project617](https://user-images.githubusercontent.com/40290711/128600217-a0864106-cb62-4709-acd7-f966b629e1c0.PNG)

- Use rsync utility to backup all the files in the log directory /var/log into /home/recovery/logs:
sudo rsync -av /var/log/. /home/recovery/logs/

![project619](https://user-images.githubusercontent.com/40290711/128600301-554d111c-d609-4ee4-a470-42498ed4d3a9.PNG)

- Mount /var/log on logs-lv logical volume:
sudo mount /dev/webdata-vg/logs-lv /var/log

![project618](https://user-images.githubusercontent.com/40290711/128600318-06a9166a-4ae5-4173-92b4-4fd2f327d0dd.PNG)

- Restore log files back into /var/log directory:
sudo rsync -av /home/recovery/logs/log/. /var/log

#### Step 2 : Update the '/etc/fstab' File.
Update /etc/fstab file so that the mount configuration will persist after restart of the server.

- The UUID of the device will be used to update the /etc/fstab file;
sudo blkid

![project620](https://user-images.githubusercontent.com/40290711/128600395-c1495576-1f0e-4518-825e-7b5cdb8cd959.PNG)

sudo vi /etc/fstab

Update /etc/fstab in this format using your own UUID and rememeber to remove the leading and ending quotes.

![project621](https://user-images.githubusercontent.com/40290711/128600529-fd25adf2-2f5d-4e72-98be-e5951b9ae2fc.PNG)

- Test the configuration and reload the daemon:
 sudo mount -a
 sudo systemctl daemon-reload
 
 ![project622](https://user-images.githubusercontent.com/40290711/128600573-c217ef36-e911-40f0-a17b-49313e5286cf.PNG)

- Verify your setup by running df -h, output must look like this:
![project623](https://user-images.githubusercontent.com/40290711/128600598-9aaa46ee-ae9d-472e-a193-3cf2fd1c141a.PNG)



