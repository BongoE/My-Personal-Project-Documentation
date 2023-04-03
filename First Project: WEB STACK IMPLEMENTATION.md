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

As the acronyms already indicate, we shall need a Linus System (The Redhat EC2 Instance has been created as pedicted above). Next is the Apache HTTP Server, Mysql Database Management System and a PHP Programming Lnaguage

### In this project, I will implement a web solution based on LAMP stack on a Linux server by implementing the steps below:


# Step 1 — Installing Apache and Updating the Firewall
* We need to Install Apache using Ubuntu’s package manager, apt:


```
$ sudo apt update

$ sudo apt install apache2
```

**To verify that apache2 is running as a Service in our OS, use following command**

```
 sudo systemctl status apache2
 ```

