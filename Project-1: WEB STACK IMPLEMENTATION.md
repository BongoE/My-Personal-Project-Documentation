# WEB STACK IMPLEMENTATION (LAMP) IN AWS

A Web Stack Implementation (LAMP) Stack in AWS. LAMP is an acronym (**L**inux, **A**pache, **M**ysql, **P**hp/**P**ython/**P**erl)
A technology stackis a set of frameworks and tools used to develop a software product. The frame work and tools are specifically chosen to work together in creating a well functioning software. This software could be a website, mobile application etc.

Create an aws account on the aws managemet console. Click here: https://aws.amazon.com/de/console/

Here is a documentation on the creation of an EC2: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html

If you are the account owner, for security purposes, I recommends that you create an IAM user for yourself for daily use of your resources.

Once you login into AWS successfully, you will see the main console with all the service listed as follows

Before proceeding with the creation of an EC2 instance, Select the desired and closest region to you.

Let me list certain parameters when choosing an AWS region which factors should you consider:

* Latency and proximity

* Security and regulatory compliance

* Service Level Agreements (SLA)

And finally, the most important is the **Cost**

I created and EC2 Instance with a RedHat AMI. Make sure the keypair grants SSH access.

**IMPORTANT - save your private key (.pem file) securely and do not share it with anyone! If you lose it, you will not be able to connect to your server ever again!**

The next phase is to view your instance state, click on your instance to view it. 
At this stage, you will see if your instance is running and the status check is **2/2 check passed**

**Next** is to connect my instance to an SSH Client. 
In my own case, i use Gitbash, Visual Studio Code and MOBAXTERM, you can decide to use PUTTY or any other existing ones.
And to connect to a remote server, you need to copy the public ip address of the instance you want to ssh into.

As the acronyms already indicate, we shall need:

```

* Linus System (The Redhat EC2 Instance has been created as pedicted above)
* Apache HTTP Server
* Mysql Database Management System 
* PHP Programming Lnaguage

```


### In this project, I will implement a web solution based on LAMP stack on a Linux server by implementing the steps below:

# step 1  Creation of a Linux Operation System
* A Linux Operating system with a RedHat AMI has beeen created with the above steps 


# Step 2  Installing Apache and Updating the Firewall
* We need to Install Apache using Ubuntu’s package manager, apt:


```
$ sudo apt update

$ sudo apt install apache2
```

**To verify that apache2 is running as a Service in our OS, use following command**

```
 sudo systemctl status apache2
 ```


Before we can receive any traffic by our Web Server, we need to open TCP port 80 which is the default port that web browsers use to access web pages on the Internet

To open our port 80, I went back to the instance, clicked on security, clicked on edit inbound rules and I add rules.
I added port 80 for http and port 443 https and I cliked on save rules
## Our server is running and we can access it locally and from the Internet (Source 0.0.0.0/0 means ‘from any IP address’).
 
* To verify and check that we can access it locally in our ubuntu shell and from the internet, run:

```
$ curl http://localhost:80
or
$ curl http://127.0.0.1:80
```


**As an output you can see some strangely formatted test, do not worry, we just made sure that our Apache web service responds to ‘curl’ command with some payload.**

Open a web browser of your choice and try to access following url


```
http://<Public-ip-address>:80
```

*  **Another way to retrieve your Public IP address, other than to check it in AWS Web Console, is to use the following command.**

```
$ curl -s http://169.254.169.254/latest/meta-data/public-ipv4


# STEP 2 - Installing MySQL

### We need to install the database system to be able to store and manage data for our website. MySQL is a popular database management system used within PHP environments.

* To install mysql-server, run:

```
sudo apt -y install mysql-server
```

* After the installation it is recommended that we run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lockdown access to our database system.
* Start the interactive Script by running:


```
sudo mysql_secure_installation
```


* This will ask if you want to configure the VALIDATE PASSWORD PLUGIN.


## Note: Enabling this feature is something of a judgment call. If enabled, passwords which don’t match the specified criteria will be rejected by MySQL with an error. It is safe to leave validation disabled, but you should always use strong, unique passwords for database credentials.



Answer Y for yes, or anything else to continue without enabling.


```
VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?
Press y|Y for Yes, any other key for No:
```

* If you answer “yes”, you’ll be asked to select a level of password validation. Keep in mind that if you enter 2 for the strongest level, you will receive errors when attempting to set any password which does not contain numbers, upper and lowercase letters, and special characters, or which is based on common dictionary words.



### <p> If we enabled password validation, we’ll be shown the password strength for the root password we just entered and our server will ask if we want to continue with that password. If we are happy with our current password, enter Y for “yes” at the prompt:</p>



### For the rest of the questions, We press Y and hit the ENTER key at each prompt. This will remove some anonymous users and the test database, disable remote root logins, and load these new rules so that MySQL immediately respects the changes we have made.



when you are done, you will get this **output:** 


<img width="714" alt="sql pluin" src="https://user-images.githubusercontent.com/85270361/123550451-66b8fc00-d765-11eb-814c-81fc9d924aea.PNG">


* When you're finished, test if you're able to login to the MySQL Console by typing:
            
