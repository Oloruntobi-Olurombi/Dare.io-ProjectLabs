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

* Both web servers can be accessed by clients using their public IP addresses.

* For the Load Balancer, we would be spinning up an Ubuntu Server 20.04 EC2 instance We would open TCP port 80, Install Apache Load Balancer on The server and configure it to point traffic coming to Load Balancer to go to both Web Servers

* As shown below, we have spun up a load balancing instance:

![image](https://user-images.githubusercontent.com/40290711/128847033-4152c1f9-774e-42c1-a005-a504c6498cf6.png)

-Install Apache Load Balancer on the load balancer instance, and configure it to point traffic coming to LB to both Web Servers by running the below commands: 

sudo apt update

sudo apt install apache2 -y

sudo apt-get install libxml2-dev

#Enable following modules:

sudo a2enmod rewrite

sudo a2enmod proxy

sudo a2enmod proxy_balancer

sudo a2enmod proxy_http

sudo a2enmod headers

sudo a2enmod lbmethod_bytraffic

#Restart apache2 service

sudo systemctl restart apache2

![image](https://user-images.githubusercontent.com/40290711/128848347-5a7e8ea6-7584-48b2-ba10-c6946ed169f4.png)

![image](https://user-images.githubusercontent.com/40290711/128848391-66e25140-6804-4a86-b7f2-42b829052ac6.png)

- Configure the Load Balancer:

Edit the 000-default.conf file using - sudo vi /etc/apache2/sites-available/000-default.conf

Add this configuration into the section <VirtualHost *:80>

<Proxy "balancer://mycluster"> BalancerMember http://:80 loadfactor=5 timeout=1 BalancerMember http://:80 loadfactor=5 timeout=1 ProxySet lbmethod=bytraffic # ProxySet lbmethod=byrequests:

    ProxyPreserveHost On
    ProxyPass / balancer://mycluster/
    ProxyPassReverse / balancer://mycluster/


![image](https://user-images.githubusercontent.com/40290711/128848718-44d04af9-7a2c-4eea-b6ea-633474153a09.png)

Then restart the apache server using:

sudo systemctl restart apache2

- Verify that the configuration works - we try accessing the load balancers public IP address or Public DNS name from our browser:

![image](https://user-images.githubusercontent.com/40290711/128848880-6f3528ad-d246-4827-b305-72f2fbb5d33b.png)


![image](https://user-images.githubusercontent.com/40290711/128848910-af54cc80-3ef1-40a7-be6e-c0510276f59d.png)


- Open two ssh/Putty consoles for both Web Servers and run following command:

sudo tail -f /var/log/httpd/access_log

Try to refresh your browser page http:///index.php several times and make sure that both servers receive HTTP GET requests from your LB - new records must appear in each server’s log file. The number of requests to each server will be approximately the same since we set loadfactor to the same value for both servers - it means that traffic will be disctributed evenly between them.

If you have configured everything correctly - your users will not even notice that their requests are served by more than one server.

![image](https://user-images.githubusercontent.com/40290711/128849047-18796eb6-d6e8-42e5-824f-e0c54e809e61.png)


![image](https://user-images.githubusercontent.com/40290711/128849079-62620b79-d64f-47c0-bc55-f5f9e7090ac1.png)

#### Configure Local DNS Names Resolution:

Sometimes it is tedious to remember and switch between IP addresses, especially if you have a lot of servers under your management. What we can do, is to configure local domain name resolution. The easiest way is to use /etc/hosts file, although this approach is not very scalable, but it is very easy to configure and shows the concept well. So let us configure IP address to domain name mapping for our LB.
Open this file on your LB server:

sudo vi /etc/hosts

- Add 2 records into this file with Local IP address and arbitrary name for both of your Web Servers

WebServer1-Private-IP-Address Web1

WebServer2-Private-IP-Address Web2

![image](https://user-images.githubusercontent.com/40290711/128849397-07a08835-6a0b-44e7-b934-10c69a30ce77.png)

- Now you can update your LB config file with those names instead of IP addresses:

sudo vi /etc/apache2/sites-available/000-default.conf

![image](https://user-images.githubusercontent.com/40290711/128849566-e946053b-5ce2-46aa-8fa0-744a5420eacf.png)

- You can try to curl your Web Servers from LB locally curl http://web1 or curl http://web2 - it shall work.

![image](https://user-images.githubusercontent.com/40290711/128849776-4ba41377-4141-4cba-9e1f-1903cdf38222.png)

##### Remember, this is only internal configuration and it is also local to your LB server, these names will neither be ‘resolvable’ from other servers internally nor from the Internet.
