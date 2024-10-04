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
- [Network Diagnostics](#network-diagnostics)
  - [Ping](#ping)
  - [Traceroute](#traceroute)
- [Useful SQL Queries](#useful-sql-queries)
- [Troubleshooting](#troubleshooting)
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
   
   !(screenshot)[hhhhhhh]

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

3. **Start MySQL service**:
   ```bash
   sudo systemctl start mysql
   ```

4. **Secure MySQL installation** (optional but recommended):
   ```bash
   sudo mysql_secure_installation
   ```

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

### Step 4: Configure MySQL Server for Remote Connections
By default, MySQL only listens to `localhost`. You need to configure it to accept connections from remote IPs.

1. **Edit MySQL configuration** on Server A:
   ```bash
   sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
   ```

2. **Find the line** with `bind-address = 127.0.0.1` and change it to:
   ```bash
   bind-address = 0.0.0.0
   ```

3. **Restart MySQL service** to apply changes:
   ```bash
   sudo systemctl restart mysql
   ```

### Step 5: Open MySQL Port on Server A Security Groups
1. **Open port 3306** (the MySQL default port) on Server A’s Security Group. 
2. **Allow access only** to Server B’s local IP address for added security.

### Step 6: Connect to MySQL Server from MySQL Client
1. From **Server B**, connect to the MySQL server on Server A:
   ```bash
   mysql -u root -p -h <Server-A-Private-IP>
   ```

2. You will be prompted for the **root password** you set earlier. Enter it to access the MySQL server.

### Step 7: Run Basic SQL Commands
Once connected, you can run SQL commands from the client on Server B to interact with the MySQL server on Server A.


```sql
SHOW DATABASES;
```
You will see an an output similar to the below image


## Conclusion
You have successfully set up a basic **Client-Server Architecture** using MySQL with two Linux-based EC2 instances. You can now execute SQL queries remotely, and the two servers can communicate over the network.

