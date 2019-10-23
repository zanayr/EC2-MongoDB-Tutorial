# Ubuntu Server and MongoDB setup tutorial  
This is a short tutorial on how to create an Ubuntu server instance using the Amazon EC2 service and setting up a MongoDB database.  

### Table of Contents  
1. [Creating an Ubuntu server instance](#creating-an-ubuntu-server-instance)
2. [Securing your Ubuntu server (very important, DO NOT skip this step)](#securing-your-ubuntu-server)
3. [Creating a MongoDB database](#creating-a-mongodb-database)

### Creating an Ubuntu server instance  
**1.** Sign into Amazon AWS account
![Sign in to amazon AWS account.][signin]
**2.** Under the **All Services** accordion, select the **EC2** link, listed under the **Compute** header.

**3.** Click the **Launch Instance** button, located under the **Create Instance** header.

**4.** Click the **Select** button of the **Ubuntu Server** list item.

**5.** Click the **Review and Launch** button in the bottom right corner, making sure that the *Free Tier* option is selected.

**6.** A modal will appear, select the *Create a New Key Pair* option from the first drop down menu. Name the key pair, for reference only, and then download the key pair. Saving the .pem file somewhere *accessible* and *memorable.*

**7.** First rename your server instance, then open a terminal client, and enter the following commands, replacing `<path>` with the path of the above .pem file downloaded in **Step 6**. The `chmod 400` command will update the file's permissions to be readonly, allowing ssh to use the key. More on chmod [here.](https://www.linode.com/docs/tools-reference/tools/modify-file-permissions-with-chmod/)

Next use ssh followed by the `-i` flag (telling ssh to use the identity of the key pair) followed by the same file path and a user name `ubuntu@<ip address>`, making sure to replace `<ip address>` with the IP address of the Ubuntu server. This is located under the **Description** tab of the selected server instance.
```
chmod 400 <path>
ssh -i <path> ubuntu@<ip address>
```
Entering `yes` when promted `The authenticity of host...`, and now terminal is connected to the instance of the Ubuntu server.
```
ubuntu@<server-ip>:~$
```


### Securing your Ubuntu server  
**1.** Navigate to your Ubuntu server instance, and click the **launch-wizard-1** link under the **Security groups** header of the **Description** tab.

**2.** Under the **Inbound** tab, click the **Edit** button.

**3.** Click the **Add Rule** button, entering in a port range of 27017, the default port for MongoDB. Select **My IP** option in the **Source** dropdown menu. Make sure that your IP address appears in the following text box. You can check your IP address by googling "what is my ip".

Your new rule should appear under the **Inbound** tab.


### Creating a MongoDB database  
**1.** After connecting terminal to the Ubuntu server instance, see **Step 7** of **Creating an Ubuntu server instance**, the required node packages will need to be installed before creating a MongoDB database. Enter the following commands, sequentially, entering `y` or `yes` to all prompts, allowing each to complete.
```
sudo apt update

sudo apt install nodejs npm mongodb-clients mongodb-server

sudo npm install -g n forever

sudo n latest
```
**2.** Enter the following command `sudo vi /etc/mongodb.conf` and navigate the terminal cursor to line 11, changing `bind_ip = 127.0.0.1` to `bind_ip = 0.0.0.0`. To be able to edit this file, hold the i key to insert, to save press the escape key and type `:wq` and enter. More can be found [here.](https://www.howtogeek.com/102468/a-beginners-guide-to-editing-text-files-with-vi/)

**3.** Finally enter the following command. It may freeze the terminal while it loads.
```
sudo service mongodb restart
```

**4.** Open the Compass application that can be downloaded [here.](https://www.mongodb.com/products/compass)

**5.** Under the **New Connection** tab on the *left* side of Compass, change the **Hostname** textbox to be the server's IP address, see **Step 7** of **Creating an Ubuntu server instance** above and the port entered into the security group created in **Step 3** of **Securing your Ubuntu server**, 27017 being the default port range.

**6.** Click the **Connect** button to connect to the MongoDB.

[signin]: foo.png
