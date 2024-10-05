# Client-Server Architecture with MySQL

This project demonstrates how to set up a **Client-Server Architecture** using **MySQL Database Management System (DBMS)**, where one server acts as the MySQL database server and another as the MySQL client.

## Table of Contents
- [Introduction](#introduction)
- [Requirements](#requirements)
- [Architecture Overview](#architecture-overview)
- [Steps to Implement](#steps-to-implement)
  - [Step 1: Launch EC2 Instances](#step-1-launch-ec2-instances)
  - [Step 2: Install MySQL Server on Server A](#step-2-install-mysql-server-on-server-a)
  - [Step 3: Install MySQL Client on Server B](#step-3-install-mysql-client-on-server-b)
  - [Step 4: Configure MySQL Server for Remote Connections](#step-4-configure-mysql-server-for-remote-connections)
  - [Step 5: Open MySQL Port on Server A Security Groups](#step-5-open-mysql-port-on-server-a-security-groups)
  - [Step 6: Connect to MySQL Server from MySQL Client](#step-6-connect-to-mysql-server-from-mysql-client)
  - [Step 7: Run Basic SQL Commands](#step-7-run-basic-sql-commands)
- [Conclusion](#conclusion)

---

## Introduction
This guide explains how to set up a MySQL client-server architecture using two **Linux-based EC2 instances** on AWS. You will install the **MySQL Server** on one instance and **MySQL Client** on another. The two instances will communicate over a network to perform SQL queries.

## Requirements
- Two **Linux-based EC2 instances** (Server A and Server B)
- AWS account
- SSH access to both servers

## Architecture Overview
- **Server A**: Acts as the **MySQL Server**.
- **Server B**: Acts as the **MySQL Client**.
- Both servers are in the same virtual private cloud (VPC), allowing communication using local IP addresses.

---

## Steps to Implement

### Step 1: Launch EC2 Instances
1. Create two EC2 instances (both Linux-based):
   - **Server A**: Name it `mysql-server`
   - **Server B**: Name it `mysql-client`

   ![screenshot](https://github.com/Prince-Tee/stegHub_Client-Server/blob/main/screenshots%20from%20my%20local%20env/launching%20myserver%20and%20myclient%20on%20ec2.PNG)

2. Assign **Security Groups** to both instances to control access:
   - Ensure both instances are in the same VPC.
   - Allow SSH access for both instances.

### Step 2: Install MySQL Server on Server A
1. **Connect to Server A** (MySQL Server):
   ```bash
   ssh -i your-key.pem ubuntu@<Server-A-IP>
   ```

2. **Update package list** and install MySQL Server:
   ```bash
   sudo apt update
   sudo apt install mysql-server -y
   ```
   !(screenshot)[hhhhhhh]
3. **Start MySQL service**:
   ```bash
   sudo systemctl start mysql
    ```
    **login to the mysql console**:
   ```
     sudo mysql
   ```
   **Set the root user password**:
    ```
    ALTER USER 'root'@'localhost' IDENTIFIED BY '<your password>';
    ```
     ![screenshot](https://github.com/Prince-Tee/stegHub_Client-Server/blob/main/screenshots%20from%20my%20local%20env/sudo%20sql_setting%20password.PNG)

4. **Secure MySQL installation** :
   ```bash
   sudo mysql_secure_installation
   ```
   ![screenshot](https://github.com/Prince-Tee/stegHub_Client-Server/blob/main/screenshots%20from%20my%20local%20env/mysql%20installation.PNG)

### Step 3: Install MySQL Client on Server B
1. **Connect to Server B** (MySQL Client):
   ```bash
   ssh -i your-key.pem ubuntu@<Server-B-IP>
   ```
   

2. **Update package list** and install MySQL Client:
   ```bash
   sudo apt update
   sudo apt install mysql-client -y
   ```
   ![screenshot](https://github.com/Prince-Tee/stegHub_Client-Server/blob/main/screenshots%20from%20my%20local%20env/mysql%20installed%20for%20mysql%20client.PNG)

### Step 4: Configure MySQL Server for Remote Connections
By default, MySQL only listens to `localhost`. You need to configure it to accept connections from remote IPs.

1. **Edit MySQL configuration** on Server A:
   ```bash
   sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
   ```
   ![screenshot](https://github.com/Prince-Tee/stegHub_Client-Server/blob/main/screenshots%20from%20my%20local%20env/nano%20into%20the%20sql%20config%20to%20chage%20the%20bind%20address.PNG)

2. **Find the line** with `bind-address = 127.0.0.1` and change it to:
   ```bash
   bind-address = 0.0.0.0
   ```
   ![screenshot](https://github.com/Prince-Tee/stegHub_Client-Server/blob/main/screenshots%20from%20my%20local%20env/bind%20address%20changed.PNG)

3. **Restart MySQL service** to apply changes:
   ```bash
   sudo systemctl restart mysql
   ```

### Open MySQL Port on Server A Security Groups
1. **Open port 3306** (the MySQL default port) on Server A’s Security Group. 
2. **Allow access only** to Server B’s local IP address for added security.
![screenshot](https://github.com/Prince-Tee/stegHub_Client-Server/blob/main/screenshots%20from%20my%20local%20env/configure%20private%20ip%20of%20mysql%20client%20in%20inbound%20rule%20of%20mysql%20server.PNG)

### Connect to MySQL Server from MySQL Client
First create a remote user named remote_user on mysql-server:

Login to MySQL database on the mysql-server with the root user.
```
sudo mysql -u root -p
```
Create a new user named remote-user and grant all privileges to the user:
```
CREATE USER 'remote_user'@'%' IDENTIFIED BY 'Taiwo123@'; GRANT ALL PRIVILEGES ON *.* TO 'remote_user'@'%' WITH GRANT OPTION; FLUSH PRIVILEGES;
```
![screenshot](https://github.com/Prince-Tee/stegHub_Client-Server/blob/main/screenshots%20from%20my%20local%20env/creating%20a%20remote%20user%20with%20priviledges.PNG)

Exit the mysql console.

exit

'''
'%' allows you to connect from any IP. It can be replaced with a specific IP for improved security.

Restart mysql.
'''
```
sudo systemctl restart mysql
```

1. From **Server B**, connect to the MySQL server on Server A:
   ```bash
   mysql -u remote_user -p -h <Server-A-Private-IP>
   ```
   ![screenshot](https://github.com/Prince-Tee/stegHub_Client-Server/blob/main/screenshots%20from%20my%20local%20env/connecting%20to%20mysql%20server%20from%20mysql%20client.PNG)

2. You will be prompted for the **root password** you set earlier. Enter it to access the MySQL server.
   ![screenshot](https://github.com/Prince-Tee/stegHub_Client-Server/blob/main/screenshots%20from%20my%20local%20env/connecting%20to%20mysql%20server%20from%20mysql%20client.PNG)

### Step 7: Run Basic SQL Commands
Once connected, you can run SQL commands from the client on Server B to interact with the MySQL server on Server A.


```sql
SHOW DATABASES;
```
You will see an an output similar to the below image
 ![screenshot](https://github.com/Prince-Tee/stegHub_Client-Server/blob/main/screenshots%20from%20my%20local%20env/SHOW%20DATABASES%3B.PNG)


## Conclusion
You have successfully set up a basic **Client-Server Architecture** using MySQL with two Linux-based EC2 instances. You can now execute SQL queries remotely, and the two servers can communicate over the network.