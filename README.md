# Ubuntu Server and MongoDB setup tutorial  
This is a short tutorial on how to create an Ubuntu server instance using the Amazon EC2 service and setting up a MongoDB database.  

### Table of Contents  
1. [Creating an Ubuntu server instance](#creating-an-ubuntu-server-instance)
2. [Securing your Ubuntu server (very important, **DO NOT** skip this step)](#securing-your-ubuntu-server)
3. [Creating a MongoDB database](#creating-a-mongodb-database)

### Creating an Ubuntu server instance  
**1.** Sign into Amazon AWS account [here.](https://aws.amazon.com/)

**2.** Under the **All Services** accordion, select the **EC2** link, listed under the **Compute** header.  
![Click on EC2 link.][1]

**3.** Click the **Launch Instance** button, located under the **Create Instance** header.

**4.** Click the **Select** button of the **Ubuntu Server** list item.

**5.** Click the **Review and Launch** button in the bottom right corner, making sure that the *Free Tier* option is selected. Then click the **Launch** button.

**6.** A modal will appear, select the **Create a New Key Pair** option from the first drop-down menu. Name the key pair, for reference only, and then download the key pair. Saving the .pem file somewhere *accessible* and *memorable.*

**7.** After renaming your server instance, for reference only, open a new terminal client, and enter the following commands, replacing `<path>` with the path of the above .pem file downloaded in the previous step. The `chmod 400` command will update the file's permissions to be read-only, allowing ssh to use the key. More on `chmod` [here.](https://www.linode.com/docs/tools-reference/tools/modify-file-permissions-with-chmod/)

Next use `ssh` followed by the `-i` flag, telling ssh to use the identity of the key pair, followed by the same file path and a user name `ubuntu@<ip address>`, making sure to replace `<ip address>` with the IP address of the Ubuntu server. This is located under the **Description** tab of the selected server instance, next to the **IPv4 Public IP** label. (e.g. `ubuntu@0.0.0.0`)
```
chmod 400 <path>
ssh -i <path> ubuntu@<ip address>
```
Entering `yes` when promted `The authenticity of host...`, and now terminal is connected to the instance of the Ubuntu server.
```
ubuntu@ip-<ip address>:~$
```


### Securing your Ubuntu server  
**1.** Navigate to your Ubuntu server instance in the AWS concole, and click the **launch-wizard** link under the **Security groups** header of the **Description** tab.  
![Click on launch-wizard link.][2]  

**2.** Under the **Inbound** tab, click the **Edit** button.

**3.** Click the **Add Rule** button, entering in a port range of **27017**, the default port for MongoDB. Select **My IP** option in the **Source** drop-down menu. Make sure that your IP address appears in the following text box. You can check your IP address by googling "what is my ip". Click the **Save** button to save your new rule.

Your new rule should appear under the **Inbound** tab and your server is now protected behind a firewall.


### Creating a MongoDB database  
**1.** After connecting terminal to the Ubuntu server instance, see **Step 7** of **Creating an Ubuntu server instance**, required node packages will need to be installed and up to date before creating a MongoDB database. Enter the following commands, sequentially, entering `y` or `yes` to all prompts, allowing each to complete before running the next command.
```
sudo apt update

sudo apt install nodejs npm mongodb-clients mongodb-server

sudo npm install -g n forever

sudo n latest
```
**2.** Enter the following command `sudo vi /etc/mongodb.conf` and navigate the terminal cursor to line 11, changing `bind_ip = 127.0.0.1` to `bind_ip = 0.0.0.0`. To be able to edit this file, hold the i key to insert, to save press the escape key and type `:wq` and enter. More can be found [here.](https://www.howtogeek.com/102468/a-beginners-guide-to-editing-text-files-with-vi/)  
![Update the bound ip address to all.][3]  
NOTE: Double check to make sure you do not create a typo in this file!

**3.** Finally enter the following command. NOTE: It may freeze the terminal while it runs.
```
sudo service mongodb restart
```

**4.** Open the Compass application that can be downloaded [here.](https://www.mongodb.com/products/compass)

**5.** Under the **New Connection** tab on the *left* side of Compass, change the **Hostname** text box to be the server's IP address, see **Step 7** of **Creating an Ubuntu server instance** above and the port entered into the security group created in **Step 3** of **Securing your Ubuntu server**, 27017 being the default port range.

**6.** Click the **Connect** button to connect to the MongoDB.

[1]: https://github.com/zanayr/EC2-MongoDB-Tutorial/blob/master/images/1.png
[2]: https://github.com/zanayr/EC2-MongoDB-Tutorial/blob/master/images/2.png
[3]: https://github.com/zanayr/EC2-MongoDB-Tutorial/blob/master/images/3.png