## Overview - DEVOPS TOOLING WEBSITE SOLUTION

### Steps:

- Launch 4 EC2 instances that will serve as the 3 Web Servers, and 1 NFS server needed for this project (Use RHEL for the 4 servers).

- Create a 15GB volume in the same AZ as your EC2 Web Server:

![image](https://user-images.githubusercontent.com/40290711/128786862-e921584a-9661-464c-9be8-f4505583f0be.png)

- Attach the volume to your EC2 Web Server instance:

![image](https://user-images.githubusercontent.com/40290711/128787036-b2c0a010-4136-4d3e-ae42-c81fd96e790d.png)

- Use the "lsblk" command to inspect what block devices are attached to the server (the new block device is xvdf):

![image](https://user-images.githubusercontent.com/40290711/128787259-22edd980-cdc5-4c01-b243-950e866cb410.png)

- Use the "gdisk" utility to create a partition on the disk:

![image](https://user-images.githubusercontent.com/40290711/128797172-4caee813-5f35-42b9-91ab-a866035f43ea.png)

- Install “LVM2”, then run the “sudo lvmdiskscan” command to check for available partitions:

![image](https://user-images.githubusercontent.com/40290711/128797269-aec7fa3c-333a-4175-b9d4-eefd5912c012.png)

- Use the "pvcreate" utility to mark the disk as a physical volume (PV) to be used by LVM and use "sudo pvs" to check:

![image](https://user-images.githubusercontent.com/40290711/128797354-cbe924a7-e65e-4f5f-91b6-4a6de7517318.png)

![image](https://user-images.githubusercontent.com/40290711/128797376-c5e5df92-f84e-4317-ad77-3818371179fa.png)

Create a volume group and then check to see if the volume group was created "sudo vgs":

![image](https://user-images.githubusercontent.com/40290711/128797493-3dcb2d62-238a-43c6-9560-da29aafe5380.png)

- Verify that everything has been created by running "sudo vgdisplay -v":

![image](https://user-images.githubusercontent.com/40290711/128797614-d9dbe33c-66ab-440f-92d2-b15486894209.png)

- Setup the NFS server:

![image](https://user-images.githubusercontent.com/40290711/128797652-454a5e41-b7df-4346-b2af-c95276c5bf56.png)

- Set up permissions that will allow the Web servers read, write and execute files on the NFS server by running the codes below:

![image](https://user-images.githubusercontent.com/40290711/128797736-e14bec10-c617-4f55-a8b4-f5c23b719b3a.png)

- Configure access to the NFS for clients within the same subnet by running "sudo vi /etc/exports" and put in your subnet as shown below:

![image](https://user-images.githubusercontent.com/40290711/128797802-7e4f2e44-05b2-4399-a610-c3f197d44524.png)

- Since this is a test, we would be opening all ports to the NFS server (not recommended)

- Create a new Ubuntu instance which would be used for our DB server (follow all steps outlined for creating an instance)

- Create a database and name it tooling:

![image](https://user-images.githubusercontent.com/40290711/128797949-c1c9d629-d2ab-461c-bfe7-ec2937bb7137.png)

- Create a database user and name it "webaccess":

![image](https://user-images.githubusercontent.com/40290711/128842677-323921c4-025c-4e03-ac18-c755448d3267.png)

- Grant permission to "webaccess" user on "tooling" database to do anything only from the webservers "subnet cidr":

Install the NFS client

![image](https://user-images.githubusercontent.com/40290711/128843059-f14dedfe-74d4-4347-819f-a5a6c01ae1c3.png)

- Mount /var/www/ and target the NFS server’s export for apps and verify that NFS was mounted successfully by running df -h: 

![image](https://user-images.githubusercontent.com/40290711/128843210-0bc5ade0-a085-468d-9f7b-139cc94812a7.png)

- Edit the /etc/fstab file so that changes will persist on the Web Server after reboot:

![image](https://user-images.githubusercontent.com/40290711/128843345-5bd478ee-ce86-465f-97f9-4ec0a3fd03b2.png)

- Install Apache:

![image](https://user-images.githubusercontent.com/40290711/128843492-b680dc53-6214-47d8-80a1-ac5028490bf1.png)

- Repeat all these steps for the other 2 web servers:

![image](https://user-images.githubusercontent.com/40290711/128843628-1dc17f2b-6ce1-41de-94b5-905deb548058.png)

 #### congratulations your just completed this objective in style.
