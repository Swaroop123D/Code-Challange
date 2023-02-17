Backgroud: 
For this project, please think about how you would architect a scalable and secure static web application in AWS.
•	Create and deploy a running instance of a web server using a configuration management tool of your choice. The web server should serve one page with the following content.

In this exercise, we will do the following:
1. Launch an EC2 instance through the AWS console. 
2. SSH into to the EC2 instance and install a web server
3. Host a static webpage on the EC2 instance

Launch an EC2 Instance

First we need to have AWS account. 
1. Sign in to the AWS console and search for "EC2". Navigate to the EC2 dashboard and click "Launch Instance".

1. Select AMI

In this step we will choose the operating system  and the basic set of software that will come preinstalled.

We need to choose a free-tier eligible Linux option  and click "Select"

2. Choose Instance Type

In this step, we will select number of CPUs, amount of memory, type of hard disk/storage, and network speed.

Choose the option marked as free tier eligible (General Purpose — t2.micro)— it will be one of the smallest, least powerful options. Click next to configure instance details.

3. Configure Instance

In this step, we will accept all of the default options. Glance at the options to get a sense of the types of things that can be done here.

Click next to add storage.

4. Add Storage

In this step, we will choose the hard drive(s), and how fast it will perform. By default, a hard drive is already attached (it is known as a root drive because it is capable of starting the operating system).
Click next to add tags.

SECURITY

Now next steps is to Configure the Security Group

1. Configure Security Group

In this step, we will choose the type of traffic that allow from the outside. we need to allow two types of traffic — SSH (so we can "log in") and HTTP (so we can view our webpage through the browser).

a. Select "Create a new security group". Enter demo for the name and description
b.Keep the SSH rule that is already listed.
c.Click "add rule". Select HTTP for the type and keep everything else as it is.

Click review and launch.

2. Review the settings for security

A pop up window will ask to select or create a key pair. A key pair is needed to securely SSH ("log in") to our new EC2 instance.

a.Select "Create a new key pair"
b.Give the Key Pair a name 
c.Click Download Key Pair
d.Click Launch Instances
e.Click View Instance to navigate back to the EC2 dashboard
f.SSH into the EC2 instance and Install a Web Server
g.here we can see  new instance listed on the EC2 dashboard. Wait until the Instance State is ‘running’.

Selecting the instance (click the button next to the instance) displays information about the instance below. 

Navigate to  terminal and do the following:

Change the permissions on our key-pair file
First, save the downloaded key-pair .pem file to a any directory . Then change the permissions with the following command. 

change permission to 
chmod 400 <path_to_key_pair_file>
3. SSH into  new EC2 instance

ssh -i <path_to_key_pair_file> ec2-user@<public_ip_from_dashboard>
Type yes to continue.

At this point, terminal is now interacting directly with  EC2 instance.
4. Elevate privileges

sudo su
5. Update all of the packages on the instance

yum update -y

6. Install an apache webserver

yum install httpd -y
7. Start the webserver

service httpd start
8. Configure the web server to restart if it gets stopped

==============================================================================================

chkconfig httpd on
Add a static HTML file (index.html) to be served
By default, the apache web server will display the index.html file found in /var/www/html directory in the root path of  website.

In this section we will create an index.html file to be served.

Navigate to the directory
cd /var/www/html 
2. Manually copy an index.html file in this directory

check content with below command 

cat index.html

Navigate back to the EC2 dashboard in the AWS console and copy the Public DNS(IPV4) of  instance and copy it. Paste that address into  browser. If all went well, we can see the html that we created as index.html

This is how we setup the web server on ec2 instance and deploy our static web page. 


======================Monitoring ====================================================
For monitoring purposes we can use cloud watch dashbaord and log metrics 

1. we need to first generate logs and create log matrics,
2. Create Cloudwatch dashboard that will read the log matrcis, and display real time data

We can setup, threshold to count the errors and send email alerts when threshold is matched. 

