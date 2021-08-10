## Objective: Tooling Website deployment automation with Continuous Integration. 

### Steps:

#### Introduction to Jenkins: 

We are going to continue from where we stopped in project 8 by building on some of the infrastructure we already created.

- Spin up an Ubuntu instance for the Jenkins server:

![image](https://user-images.githubusercontent.com/40290711/128855934-f87f662e-0334-48fb-8ae1-f2b05ea7b74a.png)

- Install JDK (since Jenkins is a Java-based application) using:

sudo apt update

![image](https://user-images.githubusercontent.com/40290711/128856138-857dbaeb-d0bc-478d-b957-8367313a2085.png)

sudo apt install default-jdk-headless

![image](https://user-images.githubusercontent.com/40290711/128856188-d3823fce-baf2-452c-ae7c-8adf92fe53d9.png)


Install Jenkins using: 

wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ >
/etc/apt/sources.list.d/jenkins.list'

sudo apt update

sudo apt-get install jenkins

![image](https://user-images.githubusercontent.com/40290711/128856447-0b17f08d-0c1c-4958-b4f6-8fce4d1c2a8d.png)

- Make sure Jenkins is up and running:

![image](https://user-images.githubusercontent.com/40290711/128856532-29bacd0f-bcbe-43f9-be31-7f5cdfc2ba17.png)

- By default Jenkins server uses TCP port 8080 - open it by creating a new Inbound Rule in your EC2 Security Group:

![image](https://user-images.githubusercontent.com/40290711/128856616-4973228c-8dd2-4143-bfb6-7eb18ccc1fd7.png)

- Perform initial Jenkins setup.

From your browser access http://Jenkins-Server-Public-IP-Address-or-Public-DNS-Name:8080

You will be prompted to provide a default admin password

![image](https://user-images.githubusercontent.com/40290711/128856720-a36ded17-dd9b-4239-9586-ec659c175ab5.png)


- Retrieve it from your server:

sudo cat /var/lib/jenkins/secrets/initialAdminPassword

Then you will be asked which plugings to install - choose suggested plugins.

Once plugins installation is done - create an admin user and you will get your Jenkins server address.

The installation is completed!

![image](https://user-images.githubusercontent.com/40290711/128856913-83a610c0-adef-4691-8c0b-10211eacd6c5.png)

- Configure Jenkins to retrieve source codes from GitHub using Webhooks:

![image](https://user-images.githubusercontent.com/40290711/128857061-d28d16d4-fa9d-4c7d-a63e-bc1eed9dbf40.png)

- Go to Jenkins web console, click “New Item” and create a “Freestyle project”.

- To connect your GitHub repository, you will need to provide its URL, you can copy from the repository itself.

- In configuration of your Jenkins freestyle project choose Git repository, provide there the link to your Tooling GitHub repository and credentials (user/password) so Jenkins could access files in the repository.

![image](https://user-images.githubusercontent.com/40290711/128857268-27f56471-e9c2-4ec6-9692-976365b0dd96.png)

- Click on "Save" at the bottom of the Jenkins page

Save the configuration and let us try to run the build. For now we can only do it manually. Click “Build Now” button, if you have configured everything correctly, the build will be successfull and you will see it under:

![image](https://user-images.githubusercontent.com/40290711/128857642-84ad5a0d-5566-4dfd-9679-b195d27bae04.png)


We can open the build and check in “Console Output” if it has run successfully. As shown below, the build ran successfully:

![image](https://user-images.githubusercontent.com/40290711/128857736-55b377dd-faa4-4203-b00a-269b8a3be9fc.png)

- But this build does not produce anything and it runs only when we trigger it manually and we would make some adjustments to fix this.

- Click “Configure” your job/project and add these two configurations:

![image](https://user-images.githubusercontent.com/40290711/128857918-a729ef25-fa84-4d16-a36f-c9e8af698183.png)

- Configure triggering the job from GitHub webhook:

![image](https://user-images.githubusercontent.com/40290711/128857990-d8572488-e275-4f96-b28e-f7fb874ef61a.png)

- Configure “Post-build Actions” to archive all the files - files resulted from a build are called “artifacts”:

![image](https://user-images.githubusercontent.com/40290711/128858065-903d561b-2e91-4bab-a563-791f772575c6.png)

- Go ahead and make some change in any file in your GitHub repository (e.g. README.MD file) and push the changes to the master branch.

We can see that a new build has been launched automatically (by webhook) and you can see its results - artifacts, saved on Jenkins server.

- By default, the artifacts are stored on Jenkins server locally as shown below:

ls /var/lib/jenkins/jobs/tooling_github/builds/<build_number>/archive/


### Configure Jenkins to copy files to NFS server via SSH

- Now we have our artifacts saved locally on Jenkins server, the next step is to copy them to our NFS server to /mnt/apps directory.

- Jenkins is a highly extendable application and there are 1400+ plugins available. We will need a plugin that is called “Publish Over SSH”.

Install “Publish Over SSH” plugin.

- On main dashboard select “Manage Jenkins” and choose “Manage Plugins” menu item:

![image](https://user-images.githubusercontent.com/40290711/128858618-4371b839-b3be-4825-ad33-b0dadfb23df3.png)

- On “Available” tab search for “Publish Over SSH” plugin and install it: 

![image](https://user-images.githubusercontent.com/40290711/128858927-a8cf3f9a-b15c-4a9a-8a01-1669d80f21ad.png)

- Configure the job/project to copy artifacts over to NFS server. On main dashboard select “Manage Jenkins” and choose “Configure System” menu item.

- Scroll down to Publish over SSH plugin configuration section and configure it to be able to connect to your NFS server:

key - Provide a private key (content of .ppk file that you use to connect to NFS server via SSH/Putty)(NB. Convert the ppk file to open ssh format, then copy the contents of the notepad file into the key input area).

Name - Any name can be used for SSH server name

Hostname - private IP address of your NFS server

Username - ec2-user (since NFS server is based on EC2 with RHEL 8)

Remote directory - /mnt/apps since our Web Servers use it as a mointing point to retrieve files from the NFS server

- Test the configuration and make sure the connection returns Success. Remember, that TCP port 22 on NFS server must be open to receive SSH connections.

Test your configuration. If you get a "success" message as shown below, you have set everything up properly:

![image](https://user-images.githubusercontent.com/40290711/128859072-e5f025bf-0094-4b02-ab40-b56a6af87c03.png)

- Save the configuration, open your Jenkins job/project configuration page and add another one “Post-build Action”:

![image](https://user-images.githubusercontent.com/40290711/128859162-f8a80d70-9334-4556-857f-fc8d495c3f4e.png)

- Configure it to send all files probuced by the build into our previouslys define remote directory. In our case we want to copy all files and directories - so we use "**". 
- If you want to apply some particular pattern to define which files to send - use this syntax.

![image](https://user-images.githubusercontent.com/40290711/128859319-2092ba0d-3443-42b6-9692-7bc8735e2c98.png)

![image](https://user-images.githubusercontent.com/40290711/128859388-a475e337-86f3-4f66-9634-ecb20b588cfa.png)

- Had to change ownership of the /mnt/apps directory as I could not find the readme.md file when i ran the cat /mnt/apps/README.md command:

![image](https://user-images.githubusercontent.com/40290711/128859482-1ba95094-7609-4b2f-901a-9b20af140ca6.png)
