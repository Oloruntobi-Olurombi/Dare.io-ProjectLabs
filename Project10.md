## Objective: Load Balancer Solution With Nginx and SSL/TLS

We would create an EC2 VM based on Ubuntu Server 20.04 LTS and name it Nginx LB (do not forget to open TCP port 80 for HTTP connections, also open TCP port 443 - this port is used for secured HTTPS connections)

![image](https://user-images.githubusercontent.com/40290711/128929531-201dd521-c53f-44ad-9181-8d59c6f04f0e.png)


![image](https://user-images.githubusercontent.com/40290711/128929571-7f984926-cafa-4fa6-a6a9-8fff3cec50c8.png)


- Update /etc/hosts file for local DNS with Web Servers’ names (e.g. Web1 and Web2) and their local IP addresses

![image](https://user-images.githubusercontent.com/40290711/128929662-b52bf3b8-9de9-402f-847f-e21bedf9e5ed.png)

- Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers by running the commands below:

sudo apt update

sudo apt install nginx

![image](https://user-images.githubusercontent.com/40290711/128930066-a7e3ba47-2ac6-4ebf-a8b9-23e93b457606.png)

![image](https://user-images.githubusercontent.com/40290711/128930104-43edf720-6a84-4650-9301-545fa41e740b.png)

- Configure Nginx LB using Web Servers’ names defined in /etc/hosts

sudo vi /etc/nginx/nginx.conf

-insert following configuration into http section

upstream myproject { server Web1 weight=5; server Web2 weight=5; }

server { listen 80; server_name www.domain.com; location / { proxy_pass http://myproject; } }

## include /etc/nginx/sites-enabled/*:

![image](https://user-images.githubusercontent.com/40290711/128933178-a4ae4130-04a9-4fd1-bae6-c4d603b63ad7.png)

- Restart Nginx and make sure the service is up and running using the commands below:

sudo systemctl restart nginx

sudo systemctl status nginx

### Register a new domain name and configure secured connection using SSL/TLS certificates

Register a new domain name with any registrar of your choice in any domain zone (e.g. .com, .net, .org, .edu, .info, .xyz or any other)

Assign an Elastic IP to your Nginx LB server and associate your domain name with this Elastic IP. When you want to associate your domain name - it is better to have a static IP address that does not change after reboot.

![image](https://user-images.githubusercontent.com/40290711/128933582-60d6e8d0-fda8-4220-a8a4-21d6d6327998.png)

Associated the elastic IP with the domain i bought by changing the DNS settings of my domain as shown below:

![image](https://user-images.githubusercontent.com/40290711/128933666-df6b89a4-d168-4add-9b87-78c6aa605e62.png)

Check that your Web Servers can be reached from your browser using new domain name using HTTP protocol - http://<your-domain-name.com> :

![image](https://user-images.githubusercontent.com/40290711/128933777-aa1fff15-fbfa-44b5-ba9f-ac3a0df77ad2.png)

- Configure Nginx to recognize your new domain name Update your "nginx.conf" with "server_name www.<your-domain-name.com>" instead of "server_name www.domain.com":

![image](https://user-images.githubusercontent.com/40290711/128933877-7c2a8846-69fa-4477-a0a4-be2a11cfab28.png)

- When I was setting up my domain, I forgot to add the "@" symbol as my hostname, so I was having issues accessing my domain.

- Install certbot and request for an SSL/TLS certificate, also make sure snapd service is active and running:

systemctl status snapd

![image](https://user-images.githubusercontent.com/40290711/128933978-4287aa07-c4d3-47f1-a931-9472d78cfb45.png)

- Install certbot

sudo snap install --classic certbot

![image](https://user-images.githubusercontent.com/40290711/128934185-e17056aa-9700-4a94-994d-610d57ae6872.png)

- Request your certificate (just follow the certbot instructions - you will need to choose which domain you want your certificate to be issued for, domain name will be looked up from nginx.conf file so make sure you have updated it:

sudo ln -s /snap/bin/certbot /usr/bin/certbot

sudo certbot --nginx

![image](https://user-images.githubusercontent.com/40290711/128934304-8d4e2864-5fa4-4a8e-bbda-b4ae53cc41c1.png)

Test secured access to your Web Solution by trying to reach

You shall be able to access your website by using HTTPS protocol (that uses TCP port 443) and see a padlock pictogram in your browser’s search string. Click on the padlock icon and you can see the details of the certificate issued for your website.

![image](https://user-images.githubusercontent.com/40290711/128934449-9798505a-baab-427a-bc76-bb36c876e16c.png)

- Set up periodical renewal of your SSL/TLS certificate. By default, LetsEncrypt certificate is valid for 90 days, so it is recommended to renew it at least every 60 days or more frequently.

- You can test renewal command in dry-run mode:

sudo certbot renew --dry-run

![image](https://user-images.githubusercontent.com/40290711/128934577-8a1a6cee-0179-4386-8e6f-7adc67539cdd.png)

- crontab -e

and add the following line -

* */12 * * * root /usr/bin/certbot renew > /dev/null 2>&1

![image](https://user-images.githubusercontent.com/40290711/128934673-783bffa2-49f7-42fe-90d3-364f007da57e.png)

##### You can always change the interval of this cronjob if twice a day is too often by adjusting schedule expression.
