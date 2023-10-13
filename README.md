# Implement a Client Server Architecture using MySQL Database Management System (DBMS)
___
## Understanding Client-Server Architecture
Client-Server refers to an architecture in which two or more computers are connected together over a network to send and receive requests between one another. In their communcation, each machine has its own role: the machine sending requests is usally referred as **Client** and the machine responding is called **Server**.

Clients and servers work together in a client-server architecture to enable communication, data exchange and the provision of services over a network. This interaction is a fundamental concept in modern computing and is used in various applications, including web browsing, file sharing and database access. 

A simple diagram of Web Client-Server Architecture is shown below:

Here's how clients and servers work together:

### 1. Client Request
* The client initiates the communication by sending a request to the server. This request typically specifies what the client needs, such as a web page, an email, a file, or a database query.

* The request includes information like the type of service required, any parameters, and other relevant data.

### 2. Server Response
* The server receives the client's request and processes it based on the service or resource requested.

* The server performs the necessary operations to generate a response, which could be data, content, or an acknowledgment.

### 3. Data Transmission
* The server sends the response back to the client over the network.

* The client receives and processes the response. This may involve rendering a web page, displaying an email, saving a file, or presenting data.

### 4. Interaction
* The client and server can engage in a back-and-forth interaction, with the client making additional requests and the server providing responses as needed.

* This interaction continues until the client's requirements are met or until the client chooses to terminate the connection.

### 5. Statelessness
* In many client-server interactions, the server is stateless, meaning it doesn't retain information about previous requests from the same client. Each request from the client is independent, and the client must include any necessary context or session information.

* However, stateful communication is also possible, where the server maintains some level of session state between requests.

In the diagram below, a machine that is trying to access a website using web browser or simply **curl** command is a client and it sends HTTP requests to a web server (Apache, Nginx, IIS or any other server) over the internet.

If we extend this concept further and add a Database Server to our architecture, we can get the picture shown below:

In this case, the Web Server has a role of the **Client** that connects and read/writes to/from a Databse (DB) Server (MySQL, MongoDB, Oracle or SQL Server) and the communication happens over a Local Network (it can also be Internet Connection but it is a common practice to place the Web Server and DB Server close to each other in a local network).

The setup on the diagram above is a typical generic Web Stack architecture (LAMP, LEMP, MEAN, MERN). This technology can be implemented with many other technologies i.e. various Web abnd DB Servers from Small Page Applications to Large and Complex Portals.


## Implementing a Client Server Architecture using MySQL Database Management System (DBMS)

The following steps are taken to implement a basic Client-Server using MySQL Relational Database Management System (RDBMS):

### Step 1: Create and configure two Linux-Based Virtual Servers (EC2 Instance in AWS)
* On the EC2 Dashboard, click on then Launch Instance button.

* On the Name Box and Amazon Machine Image, type **mysql_server** and **ubuntu** respectively then select **2** as the number of Instances you want to create.

* Select **Ubuntu Server 22.04 LTS (HVM), SSD Volume Type**  as the Amazon Machine Image.

* Click on create new key pair.

* Give the key pair name a name of your choice (i.e client-server-key), select **RSA** as the key pair type and **.pem** as the key file format then click on **Create key pair**.

* The key pair will be donwloaded to the Downloads folder on your computer.

* Click on the Launch Instance button.

* On the EC2 Dashboard, click on the Instances tab to display all the Instances on your AWS console.

* You will notice there are 2 Instances named **mysql_server**, rename one of them to **mysql_client** by clicking on the pencil icon that appears right after the name of the Instance.

* Click on the Instance ID of the **mysql_client** Instance and copy the **Private IPv4 address**.

### Step 2: Allow MySQL connection from the MySQL Client IPv4 Address on the MySQL Server
* Click on the Instance ID of the **mysql_server** Instance.

* Click on the Security tab and then click on the Security group.

* Click on Edit Inbound Rules.

* Click on the Add Rule button.

* Select **MySQL/Aurora** as the connection type and paste the **MySQL-Client IPv4 address** you copied into the Custom IPv$ adress box and click on the **save rules** button.


### Step 3: Install MySQL-Server Software on the MySQL-Server Linux Server
* Click on the Instance ID of the **mysql_server**.

* CLick on the **Connect** button and copy the hightlighted commands shown below:

* Open your terminal.

* Go to the directory (i.e. /Downloads) where the `.pem` key pair is stored using the command shown below:

```bash
cd Downloads
```

* Paste the following command to give read permissions to the `.pem` key pai file:

```bash
sudo chmod 400 <private-key-pair-name).pem
```

* SSH into the MySQL-Server Instance using the command shown below:

```bash
ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>
```

* Update the list of packages in the package manager.

```bash
sudo apt update
```

* Run the MySQL-Server package installation.

```bash
sudo apt install mysql-server -y
```

In MySQL, the bind address parameter in the `mysqld.cnf` file is used to specify the IP address on which the MySQL server should listen for incoming connections. Its default setting is `bind-address  = 127.0.0.1`.

In other to listen to all connections from all avaialable network interfaces, the default bind address parameter is changed to `0.0.0.0` (i.e. Default Gateway). 

However, it is best practce to set security precautions such as **Firewall Rules**, **MySQL User Privileges** and in our case, we set security precautions by setting the **Inbound Rules** on the **MySQL TCP port** to only allow conections from the **MySQL-Client** on the **MySQL-Server**.

* Run the following command to change the **bind address**.

```bash
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf 
```
* Log into the MySQL console by ruuning the command shown below:

```bash
sudo mysql
```

### Step 4: Install MySQL-Client Software on the MySQL-Client Linux Server

### Step 5:

GRANT ALL PRIVILEGES ON *.* TO 'donald'@'%' WITH GRANT OPTION;

CREATE USER 'donald'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
