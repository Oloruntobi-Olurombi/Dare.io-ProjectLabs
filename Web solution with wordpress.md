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

#### Step 3 --- Prepare the Database Server:
Launch a second RedHat EC2 instance that will have a role – ‘DB Server’
Repeat the same steps as for the Web Server, but instead of apps-lv create db-lv and mount it to /db directory instead of /var/www/html/.

- Create 3 volumes in the same AZ as your Database Server EC2, each of 10 GiB.
![project624](https://user-images.githubusercontent.com/40290711/128600736-27c8a9af-03b7-47a7-b540-3285accbef6a.PNG)

- Attach all three volumes one by one to the Database Server EC2 instance:
![project625](https://user-images.githubusercontent.com/40290711/128600821-be7ac28b-451c-43aa-9010-c0a3fb5d3f20.PNG)

-  Check if the three volumes are attached to the server via the command line using: 
lsblk 

![project626](https://user-images.githubusercontent.com/40290711/128600852-60ef4ce9-554a-4dbe-849a-9022b9b639e1.PNG)

- sudo gdisk /dev/xvdg:
![project627](https://user-images.githubusercontent.com/40290711/128600895-2dce4dd0-2fa4-46b1-971b-fb7683aa78ac.PNG)

![project627b](https://user-images.githubusercontent.com/40290711/128600901-cad2bf78-1749-471f-8265-67c4718bc3a2.PNG)

- sudo gdisk /dev/xvdh:

![project628](https://user-images.githubusercontent.com/40290711/128600928-4bd3822a-74f9-4d0b-95fa-370f1a597e52.PNG)

![project628b](https://user-images.githubusercontent.com/40290711/128600937-8d0c7fc2-ee4c-4f26-bb34-1b58e93826b8.PNG)

- To view the newly configured partition on each of the 3 disks using:
lsblk

![project629](https://user-images.githubusercontent.com/40290711/128600982-b09b4432-c5d3-4d70-9833-23c747acbb86.PNG)

- Install lvm2 package using:
sudo yum install lvm2

![project630](https://user-images.githubusercontent.com/40290711/128601034-f7e2f08e-4b54-4e2b-9dd4-778fc71f6a4f.PNG)

- Mark each of 3 disks as physical volumes (PVs) to be used by LVM using:
sudo pvcreate /dev/xvdf1 /dev/xvdg1 /dev/xvdh1

![project631](https://user-images.githubusercontent.com/40290711/128601066-5a9105b7-3c8d-4b8c-adf2-a4a4475d31d1.PNG)

- To add all 3 PVs to a volume group (VG) use:
sudo vgcreate vg-database /dev/xvdh1 /dev/xvdg1 /dev/xvdf1 

![project632](https://user-images.githubusercontent.com/40290711/128601135-7c12663c-8f98-4c10-b2b5-91c0f8716c1d.PNG)

- Verify that the VG has been created successfully by running:
sudo vgs

![project633](https://user-images.githubusercontent.com/40290711/128601195-c56131d3-9977-4e8f-aa54-e33b12c61a68.PNG)


- Verify that the Logical Volume has been created successfully by running:
 sudo lvcreate -n db-lv -L 20G vg-database 
 
 ![project634](https://user-images.githubusercontent.com/40290711/128601354-dcd64c74-6276-45a4-bc39-7a914581367e.PNG)

- Create a new directory called "db":
sudo mkdir /db 
 
- Format the logical volumes with ext4 filesystem using:
sudo mkfs -t ext4 /dev/vg-database/db-lv

![project635](https://user-images.githubusercontent.com/40290711/128601432-14dda95d-13f2-454b-b040-9446426a70aa.PNG)

- Mount the database using:
sudo mount /dev/vg-database/db-lv /db

![project636](https://user-images.githubusercontent.com/40290711/128601472-707c0c15-e34c-4c2d-9356-d6b78aa1a675.PNG)

![project637](https://user-images.githubusercontent.com/40290711/128601475-f3ab3266-b1bd-4a99-9f7d-79620fec7b53.PNG)


#### Step 4: Install WordPress on your Web Server EC2
- Update the repository using:
sudo yum update -y

![project638](https://user-images.githubusercontent.com/40290711/128601582-8fc2d583-cf4d-4bd0-a4db-20bd353d1beb.PNG)

- Install wget, Apache and it’s dependencies
sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json

![project639](https://user-images.githubusercontent.com/40290711/128601604-6f4b317e-187e-45d3-8423-2ef52f4e15bf.PNG)

- Start Apache
sudo systemctl enable httpd
sudo systemctl start httpd

![project641](https://user-images.githubusercontent.com/40290711/128601636-a1a816da-acaa-43b7-8d39-5ee5ab5bdf1a.PNG)

- To install PHP and it’s depemdencies:

sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
sudo yum module list php
sudo yum module reset php
sudo yum module enable php:remi-7.4
sudo yum install php php-opcache php-gd php-curl php-mysqlnd
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
setsebool -P httpd_execmem 1

![project642](https://user-images.githubusercontent.com/40290711/128601696-0aa051a8-2e40-461f-8345-7645cc958393.PNG)

- Restart Apache
sudo systemctl restart httpd

- Download wordpress and copy wordpress to var/www/html
mkdir wordpress

![project643](https://user-images.githubusercontent.com/40290711/128601834-841865d8-67c2-490e-99b7-6f500e5ca3e2.PNG)

cd   wordpress
sudo wget http://wordpress.org/latest.tar.gz

![project644](https://user-images.githubusercontent.com/40290711/128601847-39cc9a6b-c39f-4541-bc9a-d55ca8a36646.PNG)

sudo tar xzvf latest.tar.gz
sudo rm -rf latest.tar.gz
cp wordpress/wp-config-sample.php wordpress/wp-config.php
![project645](https://user-images.githubusercontent.com/40290711/128601868-b4324a32-0cf4-4876-8484-f8e7d04b2022.PNG)
cp -R wordpress /var/www/html/

- Configure SELinux Policies:
sudo chown -R apache:apache /var/www/html/wordpress
sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
sudo setsebool -P httpd_can_network_connect=1
  
#### Step 4 — Install MySQL on your DB Server EC2
sudo yum update
sudo yum install mysql-server

![project646](https://user-images.githubusercontent.com/40290711/128601938-a88c48b1-4c8a-4d45-8b5e-7b5bae2fadfd.PNG)

- Verify that the service is up and running by using sudo systemctl status mysqld, if it is not running, restart the service and enable it so it will be running even after reboot:

sudo systemctl restart mysqld
sudo systemctl enable mysqld

![project647](https://user-images.githubusercontent.com/40290711/128601993-19d0c038-2794-47c6-8cbb-0089e1479fc1.PNG)

![project650](https://user-images.githubusercontent.com/40290711/128602059-d984217a-80f2-490a-927f-23e824873209.PNG)


#### Step 5 — Configure DB to work with WordPress
![project651](https://user-images.githubusercontent.com/40290711/128602071-e47dfbac-97b7-48d3-b6bf-ad8e9e88564f.PNG)

sudo mysql
CREATE DATABASE wordpress;

![project652](https://user-images.githubusercontent.com/40290711/128602101-e075121a-aa08-44e1-bba7-f28d17865983.PNG)

![project653](https://user-images.githubusercontent.com/40290711/128602115-3b10379b-b98a-4d6c-8df4-d40acbd200f7.PNG)

CREATE USER `myuser`@`<Web-Server-Private-IP-Address>` IDENTIFIED BY 'mypass';
![project655](https://user-images.githubusercontent.com/40290711/128602270-1738a2b9-9c63-4bc2-b40f-da9fb5ec000c.PNG)


GRANT ALL ON wordpress.* TO 'myuser'@'<Web-Server-Private-IP-Address>';
![project656](https://user-images.githubusercontent.com/40290711/128602277-cb7a02a9-fa36-473a-8bf8-100e5fc2800e.PNG)


FLUSH PRIVILEGES;
 
SHOW DATABASES; 
 
![project657](https://user-images.githubusercontent.com/40290711/128602299-f59a1966-0fb5-4ef0-84c8-4b43cc0c2f2a.PNG)
 
exit
 
![project658](https://user-images.githubusercontent.com/40290711/128602311-170e5539-7c8e-4cff-b22a-698ec39327b5.PNG)
 
 #### Step 7: 
 
 Restart the web server mysql using:
 sudo systemctl restart mysqld
 sudo systemctl enable mysqld
 sudo systemct1 status mysqld
 
 ![project659](https://user-images.githubusercontent.com/40290711/128602529-03dbe896-0f0f-44b4-9ba2-97d85ce37512.PNG)

 - Edit the wp-config.php using:
 sudo vi wp-config.php
 
 ![project660](https://user-images.githubusercontent.com/40290711/128602576-4e925c46-602d-4af9-94a3-ea7a4f93a204.PNG)
 
![project661](https://user-images.githubusercontent.com/40290711/128602582-1f30ee67-2f2d-42ca-b7b7-53a228aa74d0.PNG)
 
- Restart the web server mysql
![project662](https://user-images.githubusercontent.com/40290711/128602768-e3bc4445-b9fe-4d05-a52e-a7a7a6cf0ca6.PNG)
 
 - Connect the private IP address with the wordpress:
 
 ![project664](https://user-images.githubusercontent.com/40290711/128602863-12c7a4dc-5f73-4cb6-807b-c51533ccc7c8.PNG)

-SHOW DATABASES;
 ![project665](https://user-images.githubusercontent.com/40290711/128602920-77edfb3e-0433-424a-bca7-a773c45e0458.PNG)

 Try to access from your browser the link to your WordPress http://<Web-Server-Public-IP-Address>/wordpress/

![project670](https://user-images.githubusercontent.com/40290711/128602968-b4ffe9a8-d27c-430e-b9f5-a7fa5d6da206.PNG)
 
 ![project671](https://user-images.githubusercontent.com/40290711/128602976-ba214feb-48cd-435b-8394-090e0720b332.PNG)

 
####### Important: Do not forget to STOP your EC2 instances after completion of the project to avoid extra costs.

###### CONGRATULATIONS!
You have learned how to configure Linux storage susbystem and have also deployed a full-scale Web Solution using WordPress CMS and MySQL RDBMS!
 
