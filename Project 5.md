# Understanding Client-Server Architecture
- Client-Server refers to an architecture in which two or more computers are connected together over a network to send and receive requests between one another.

- In their communication, each machine has its own role: the machine sending requests is usually referred as "Client" and the machine responding (serving) is called "Server".

### A simple diagram of Web Client-Server architecture is presented below:
![image](https://user-images.githubusercontent.com/40290711/126867426-ae6bdc07-1e46-4315-a04a-532714a0d30e.png)

### Lets take a very quick example and see Client-Server communicatation in action.

##### Steps:

- Open up the Ubuntu or Windows terminal and run curl command:

 Note: If your Ubuntu does not have ‘curl’, you can install it by running sudo apt install curl
 
 ![projectclient](https://user-images.githubusercontent.com/40290711/126867609-d26a7c40-abda-412b-9308-d2648ced87b4.PNG)
 

 curl -Iv www.propitixhomes.com
 
 ![projectclient2](https://user-images.githubusercontent.com/40290711/126867675-fe2f6f9a-8dee-4b7a-8050-fd4cb44efdfe.PNG)

![projectclient3](https://user-images.githubusercontent.com/40290711/126867684-bd99cd41-b60c-44e7-93ea-cc92f3135200.PNG)

  
 In this example, your terminal will be the client, while www.propitixhomes.com will be the server.

See the response from the remote server in below output. You can also see that the requests from the URL are being served by a computer with an IP address 160.153.133.153 on port 80. More on IP addresses and ports when we get to Networking related projects.

### Nice One, you just implemented the Client-Server communication Process. You must be really proud of your achievement. See you at the top mate. Best of luck





 

