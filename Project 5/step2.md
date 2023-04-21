# IMPLEMENT A CLIENT SERVER ARCHITECTURE USING MYSQL DATABASE MANAGEMENT SYSTEM (DBMS).

## TASK 
To demonstrate a basic client-server using MySQL Relational Database Management System (RDBMS), follow the below instructions

I used 2 Instances. Deployed the instances in thesame Availability Zone (AZ) such that they can communicate with each other with the local IP Addresses alone.I deploeyd mine in  "us east 1a"


After installing mysql on both instances, we can now initiate connection through their local IP, since they are found in the same AZ.
For this to happen, we have to open port 3306 (Mysql-Aurora) on the instance-server security..see image. Go to security -> inbound-rule _> edit -> add rule. Don't forget to add the ip address of the source (Client). the command "ip add show" will display the ip address 
The server and client are now set. We donot have a user nor a database. we need to create those
Go to the server instance
1. Create and configure two Linux-based virtual servers (EC2 instances in AWS). Named the instanes to avoid ambiguity.

```
Server A name - `mysql server`
Server B name - `mysql client`
```
There are ways to ssh into an Instance. I used **Powershell**. You could use ***gitbash*** , ***mobaxterm*** or ****vscode****

It is good practice to first update the instance before any installation. Staying updated minimises trouble shooting

```
sudo apt update -y   ['-y' is to indicate that all the uodates should be done at once]
```

2. On mysql server Linux Server install MySQL Server software.

The command **sudo apt install mysql** This command did not work, so i checked on the documentation on ***mysql.com*** and found the right command
```
"sudo apt install mysql-server"
```
Side input on mysql. MySQL is a fast, multi-threaded, multi-user, and robust SQL database server. It is intended for mission-critical, heavy-load production systems and mass-deployed software.

Let us enable the service.. service enabled.

REPEAT thesame process for the client server. This time, install mysql client

3. On mysql client Linux Server install MySQL Client software.

First update the insatance with command
```
"sudo apt update -y"
```
Followed by the command 
```
"Sudo apt install mysql-client"
```

Interesting fact: MySQL is an open-source relational database management system. Its name is a combination of "My", the name of
co-founder Michael Widenius’s daughter, and "SQL", the abbreviation for Structured Query Language.



4. By default, both of your EC2 virtual servers are located in the same local virtual network, so they can communicate to each other
using local IP addresses. Use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by 
default, so you will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. 
For extra security, do not allow all IP addresses to reach your ‘mysql server’ – allow access only to the specific local
IP address of your ‘mysql client’.

5. You might need to configure MySQL server to allow connections from remote hosts.

```
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```

Replace ‘127.0.0.1’ to ‘0.0.0.0’ like this:


![5018](https://user-images.githubusercontent.com/85270361/210136418-f4832b77-89d4-4e65-8287-6e73a338a65a.PNG)


6. From mysql client Linux Server connect remotely to mysql server Database Engine without using SSH. You must use the mysql 
utility to perform this action.

7. Check that you have successfully connected to a remote MySQL server and can perform SQL queries:

```
Show databases;
```

If you see an output similar to the below image, then you have successfully completed this project – you have deloyed a fully 
functional MySQL Client-Server set up.
Well Done! You are getting there gradually. You can further play around with this set up and practice in creating/dropping databases 
& tables and inserting/selecting records to and from them.

Congratulations!


![5020](https://user-images.githubusercontent.com/85270361/210136618-f3738310-258f-4848-ab6d-710a06cce2e8.PNG)

