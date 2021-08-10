## Objective: Load Balancer Solution With Apache

This is a continuation from project 7, as we are building upon the infrastructure we had already setup in that project.

#### STEPS:

- Check to see that Apache is running on both web servers:

![image](https://user-images.githubusercontent.com/40290711/128846003-bd8fc5cf-dc53-426c-9725-685e2cf26dac.png)

![image](https://user-images.githubusercontent.com/40290711/128846391-de4d86b3-77ac-497a-8c35-fa4f5cdfc28a.png)

- Verify that the contents of the /mnt/apps directory on the NFS server are the contents present in /var/www directory of the web servers:

NFS:

![image](https://user-images.githubusercontent.com/40290711/128846513-127fbdd6-9572-42d8-b1c7-ecaa795c31a3.png)

Web Server 1:

![image](https://user-images.githubusercontent.com/40290711/128846604-f827b7c8-426c-4f0e-8465-10aa9ac13acd.png)

Web Server 2:

![image](https://user-images.githubusercontent.com/40290711/128846708-8ecf1223-896a-453a-a9e0-e01cf80c9cfe.png)

* All the necessary TCP/UDP ports have also been configured on the AWS console for the NFS server, all web servers, and the DB server.

*Both web servers can be accessed by clients using their public IP addresses.

*For the Load Balancer, we would be spinning up an Ubuntu Server 20.04 EC2 instance We would open TCP port 80, Install Apache Load Balancer on The server and configure it to point traffic coming to Load Balancer to go to both Web Servers

*As shown below, we have spun up a load balancing instance
