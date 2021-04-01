# Udacity Cloud Devops Engineering Project 2

This repository contains AWS Cloudformation template to deploy the Udagram website onto AWS.

## Description

The website is built on highly available infrastructure in AWS.

The Cloudformation service will be used to deploy the entire IT infrastructure which consists of:
networking components such as Virtual Private Cloud, Elastic Load balancer, Auto-scaling group, public and private subnets, NAT instances, bastion host, and web servers.  
Apache webserver will be installed on the web server and website code will be deployed from S3 bucket.


![network diagram]h(ttps://raw.githubusercontent.com/flyingkeyboard/cloud_devops_project2/main/First%20Draft%20of%20AWS%20diagram-1.jpeg)


### Dependencies

Below are requirements to run Cloudformation to deploy the web site and all networking infra to AWS.


Require AWS account with Administrative privileges to create VPC, IAM role, Subnets, Route Tables, Routes,  EC2, Load balancers, Autoscaling group NAT instances etc.

Require AWS API keys use AWS CLI to create stack using cloudformation template.   

Require AWS CLI tool and  AWS API credential.

AWS Access Key ID,AWS Secret Access Key,Default region name,Default output format

The Bastion host requires a EC2 key pair named BastionKeyPair 
The Web server requires a EC2 key pair named WebserverKeyPair

### Installing

Clone this reposition to local folder.

Install AWS CLI to your prefer operating system.  Refer to 
https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html

After AWS CLi is installed.  Configure the AWS CLi credential using command below:

aws configure



### Executing program

There are two Cloudformation template files and two json parameter files.  

The Cloudformation template is a text file that describes a stack, a collection of AWS resources. 
The json parameter files contain various parameters used by the template.

# Network components

The network.yml contains Cloudformation template for VPC, Subnets, NAT gateway, Route Tables, Routes.

network.yml 
network-parameters.json

# Server components:

The server.yml contains Cloudformation template for bastion server, webservers, load balancer, auto scaling groups.

server.yml
server-parameters.json


To deploy the infrastructure for the web application, please ensure that the network.yml has created the stack succesfully before running the server.yml template.   

The us-west-2 region is shown in the example below.  Other aws region can be used instead.

Execute the following command to create network infra:

aws cloudformation create-stack --stack-name stack-udc-network --template-body file://network.yml  --parameters file://network-parameters.json --capabilities "CAPABILITY_IAM" "CAPABILITY_NAMED_IAM" --region us-west-2

Go to the Cloudformation web console to check the status of the stack creation.  Under the events tab to check if there is any errors.

Then execute the following command to create all server components:

aws cloudformation create-stack --stack-name stack-udc-server --template-body file://server.yml  --parameters file://server-parameters.json --capabilities "CAPABILITY_IAM" "CAPABILITY_NAMED_IAM" --region us-west-2


# Post install 

After the infra is created successfully, go the Cloudformation page in the AWS console.  Check the stack "stack-udc-server".  In the Outputs tab to check the Load balancer URL name.  

Open the web browser to the Load balancer URL.



