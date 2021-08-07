# Objective: Web Soltion With Wordpress

### LAUNCH AN EC2 INSTANCE THAT WILL SERVE AS "WEB SERVER".

#### Step 1 : Prepare a Web Server
- Launch an EC2 instance that will serve as "Web Server". Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.

![image](https://user-images.githubusercontent.com/40290711/128598774-3bd7b553-9f26-4b90-b5d6-e8d879e0121e.png)
- Attach all three volumes one by one the Web Server EC2 instance:
![Project62](https://user-images.githubusercontent.com/40290711/128598840-b982f345-05a1-4879-98a3-4088eb1b57c6.PNG)

- Open up the linux terminal to begin configuration
- 
![project61](https://user-images.githubusercontent.com/40290711/128599072-a624d0b1-c208-4bb9-9246-3ba4196e7d01.PNG)

- Check if the three volumes are attached to the server via the command line using: 

lsblk 

![project63](https://user-images.githubusercontent.com/40290711/128598950-e4fc6bd9-b8de-437a-9e3d-cf29d1781dde.PNG)

- Use df -h command to see all mounts and free space on the server

![project64](https://user-images.githubusercontent.com/40290711/128599113-85b2be22-86a7-4522-8b77-ca13832fa505.PNG)

- Create a single partition on each of the 3 disks using:

sudo gdisk /dev/

![project64](https://user-images.githubusercontent.com/40290711/128599186-167303d5-ab90-4cc7-87ea-7fb238aab9aa.PNG)

#### Step 2 : Update the '/etc/fstab' File.
- The UUID of the device will be used to update the /etc/fstab 




