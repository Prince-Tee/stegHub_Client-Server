# Self-Study Documentation: Client-Server Architecture with MySQL

## Overview

This project aimed to set up a **Client-Server Architecture** using **MySQL Database Management System (DBMS)**. The core objective was to install **MySQL Server** on one EC2 instance (acting as the server) and **MySQL Client** on another (acting as the client), allowing the two instances to communicate over a private network for SQL-based interactions. This project helped me understand server configuration, network communication, and SQL command execution in a real-world setup.

---

## Learning Objectives
1. Understand and configure **MySQL Client-Server Architecture**.
2. Use **AWS EC2 instances** to simulate a client-server setup.
3. Install and configure **MySQL Server** and **MySQL Client**.
4. Establish remote connectivity between the server and client.
5. Perform basic **SQL queries** such as `CREATE`.
6. Read **ping** and **traceroute**.
7. Troubleshoot issues related to MySQL password policies and remote connections.

---

## Steps Taken and Reflections

### 1. Setting Up the EC2 Instances
#### Task
- Launch two **Linux-based EC2 instances** on AWS, one acting as the **MySQL Server** and the other as the **MySQL Client**.

#### Reflection
- I learned how to create and configure EC2 instances with appropriate **Security Groups** to control network traffic.
- Ensuring that both EC2 instances were in the same **VPC** was crucial for direct communication.
- **Takeaway**: Understanding EC2 instance configuration and the significance of network boundaries (such as VPCs and Security Groups) is vital for secure communication.

### 2. Installing MySQL Server on Server A
#### Task
- Install **MySQL Server** on the EC2 instance acting as the server.
- Start the MySQL service and ensure it is active.

#### Reflection
- The installation was straightforward using `sudo apt install mysql-server`.
- Starting the MySQL service and verifying its status taught me about basic service management in Linux.
- **Takeaway**: I now have a clear understanding of managing MySQL services, and the importance of regularly checking their status using `systemctl status`.

### 3. Configuring MySQL for Remote Connections
#### Task
- By default, MySQL only listens to `localhost`, so I modified the **bind-address** to `0.0.0.0` to allow remote connections.

#### Reflection
- This configuration taught me about network binding, allowing me to control which IP addresses can access MySQL.
- Using `nano` to edit `/etc/mysql/mysql.conf.d/mysqld.cnf` was an excellent way to ensure ease of access and minimize confusion during configuration.
- **Takeaway**: The `bind-address` parameter is key to allowing remote connections. Configuring this correctly, along with the right security settings, is essential for a working setup.

### 4. Setting Up Security Groups for MySQL Port
#### Task
- I opened **port 3306** in the **MySQL Server's Security Group** to allow only the **MySQL Client** IP address to connect securely.

#### Reflection
- This step emphasized the importance of securing databases by restricting access to known IPs, preventing external threats.
- Learning how to create **Security Group rules** for specific ports, especially for databases, has increased my understanding of AWS security.
- **Takeaway**: Security configuration is an ongoing process. It's critical to balance accessibility with security to protect sensitive services like MySQL.

### 5. Installing MySQL Client on Server B
#### Task
- Install **MySQL Client** on Server B to interact with the MySQL Server.

#### Reflection
- Installation of MySQL Client was quick, and the `mysql` utility provided an easy-to-use interface for connecting to the MySQL Server.
- **Takeaway**: MySQL Client provides a lightweight solution for executing SQL queries remotely. I learned that clients don’t need a full MySQL Server installation to communicate with the server.

### 6. Connecting the Client to the Server
#### Task
- Use the **MySQL Client** to connect to the **MySQL Server** using the server's private IP.

#### Reflection
- Successfully connecting to the MySQL server remotely was a breakthrough. It reinforced my understanding of networking in cloud environments, particularly VPC communication.
- This step involved configuring the **root password** and adjusting settings to allow remote connections.
- **Takeaway**: Setting up and maintaining secure passwords is essential, and adjusting MySQL configuration to allow remote access without compromising security is key.

### 7. Troubleshooting MySQL Password Policies
#### Challenge
- When I tried to use a simple password (`password`), MySQL enforced its **password policy**, rejecting weak passwords.

#### Resolution
- I resolved this by creating a stronger password and later modifying the root password using:
  ```sql
  ALTER USER 'root'@'localhost' IDENTIFIED BY 'Taiwo';
  ```

#### Reflection
- MySQL’s password validation plugin can enforce strong password policies, which I initially found restrictive but ultimately beneficial for security.
- **Takeaway**: MySQL’s password strength requirements ensure better security practices. Working around these policies requires awareness of best security practices.

### 8. Running SQL Queries from the Client
#### Task
- After establishing the connection, I ran basic SQL queries such as `SHOW DATABASES;`, 

#### Reflection
- **Takeaway**: Knowing how to perform these basic SQL queries remotely via the MySQL Client adds value to database management tasks.



---

## Troubleshooting Insights

### 1. Password Issues
- I initially encountered a password complexity issue while setting the MySQL root password (`password`). This was resolved by setting a stronger password.

### 2. Remote Connection Problems
- I had to reconfigure the MySQL bind-address and Security Group rules to allow remote connections.
- I also had to troubleshoot `mysql_secure_installation` and ensure the MySQL service was listening to the correct address (`0.0.0.0`).

---

## Conclusion

This project deepened my understanding of setting up **Client-Server Architecture** using MySQL. I learned how to install, configure, and secure MySQL, perform network diagnostics, and troubleshoot common issues related to password policies and remote connections. 

---

## Future Improvements
1. **Advanced Security**: Implement SSL/TLS encryption for MySQL communication.
2. **Automation**: Use scripts to automate EC2 instance creation and MySQL setup.
3. **Performance**: Explore MySQL optimization techniques for faster query execution.

---