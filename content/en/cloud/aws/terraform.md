---
title: "Deploy Graviton based EC2 instances with Terraform"
type: docs
weight: 3
hide_summary: true
description: >
    Learn how to Deploy Graviton based EC2 instances with Terraform.
---

# How to deploy Graviton based EC2 instances with Terraform
   This guide will help you to Create your first AWS EC2 using terraform.
   When it comes to IAC(Infrastructure As Code) Terraform is always the first choice of DevOps although there are many alternatives available in the market such as        Ansible, Chef, Puppet, SaltStack, CloudFormation but due the fact that -
   
   1. Terraform is really easy install
   2. Terraform has very good API documentation
   3. It is widely adopted in the DevOps community
   4. Great support for a popular cloud service provider such as Google Cloud Platform, AWS.

## Prerequisite
   The only prerequisite is - You must [install Terraform](https://www.terraform.io/cli/install/apt) before jumping to AWS instance Setup.
   
## Table of Content
   1. Setup AWS Account
   2. Generate Access keys (access key ID and secret access key)
   3. Generate key-pair(public key, private key) using ssh keygen
   4. Create your first Terraform infrastructure (main.tf)
   5. Terraform commands

## 1. Setup AWS Account
   As our aim of this article to setup an AWS EC2 instance the first step would be to create an AWS account.
   If you are a beginner and want to learn the Terraform then AWS provides you free tier - 12 months or 750 Hours/month, where you can experiment.

### 1.1 Sign up for AWS account
   Goto https://aws.amazon.com/ and click on Complete sign-up
      
   ![image](https://user-images.githubusercontent.com/87687468/190132437-19f68942-53b5-4837-9706-31a777d0f930.png)
      
   You need to choose AMAZON EC2 Free Tier. This tier is sufficient enough for learning purposes.
      
   ![image](https://user-images.githubusercontent.com/87687468/190132636-1312ee45-f68d-46d2-bddb-c4285816ca1a.png)

   Amazon will ask for your Credit card number to complete the sign-up process, AWS will debit around 1$ so that they can verify your card details. Amazon will            refund the amount after the authorization.

### 1.2 After Signup LogIn as ROOT user
   
   After signup, you need to log in as a ROOT user for your AWS account.
      
   ![image](https://user-images.githubusercontent.com/87687468/190132990-5fe0f4b9-5808-4bff-a321-b1d72b9bc2d5.png)
      
   Once you login into your AWS account you should see below dashboard
      
   ![image](https://user-images.githubusercontent.com/87687468/190133678-18fe9da6-e7f5-4e78-aeb2-35386bbb17cd.png)
      
      
## 2. Generate Access keys (access key ID and secret access key)
   
   Terraform installed on your Desktop/Laptop needs to communicate with AWS and to make this communication terraform needs to be authenticated.
   For authentication, we need to generate Access Keys (access key ID and secret access key). These access keys can be used for making - programmatic calls to AWS from    the AWS CLI, Tools for PowerShell, AWS SDKs, or direct AWS API calls.
   
### 2.1 Goto My Security Credentials
   
  ![image](https://user-images.githubusercontent.com/87687468/190137370-87b8ca2a-0b38-4732-80fc-3ea70c72e431.png)

### 2.2 On Your Security Credentials page click on create access keys (access key ID and secret access key)
   
  ![image](https://user-images.githubusercontent.com/87687468/190137925-c725359a-cdab-468f-8195-8cce9c1be0ae.png)
   
### 2.3 Copy the Access Key ID and Secret Access Key 

  ![image](https://user-images.githubusercontent.com/87687468/190138349-7cc0007c-def1-48b7-ad1e-4ee5b97f4b90.png)


## 3. Generate key-pair(public key, private key) using ssh keygen

### 3.1 Generate the public key and private key
Before you start playing with AWS console and terraform script we need to first generate the key-pair(public key, private key) using ssh-keygen.
Later we are going to associate both public and private keys with AWS EC2 instances.

Let us generate the key pair using the following command:
 
      $ ssh-keygen -t rsa -b 2048
       

By default, the above command will generate the public as well as private key at location '/home//.ssh'
But we can override the end destination with a custom path. (I have assigned my custom path /home/ubuntu/aws/ followed my key name .i.e. aws_key )
Here is the output along with a screenshot my terminal-
      
![image](https://user-images.githubusercontent.com/87687468/192259698-8219d63c-e28b-41e2-a67c-7f77dff20e9a.png)
      
### 3.2 Verify the generated public key and private key
In the previous step, we have generated the key-pair which we are going to use for provisioning the EC2 instance. But let us take a look at the keys and how it        looks.

   If you remember in the previous step we have generated the keys at path /home/ubuntu/aws/ we should see two key files over there -

   1. aws_key (private key)
   2. aws_key.pub (public key)
   
We are going to use public key aws_key.pub inside the terraform file to provision/start the ec2 instance.
Alright, now we have the public key and the private key with us, let us create our terraform configuration file using the public key .i.e. aws_key.pub

## 4. Create your first Terraform infrastructure (main.tf)
   Before we start writing terraform script, the first thing to learn over here is - "You need to save your configuration with .tf extension". We will start by            creating an empty main.tf file.
   
### 4.1 Provider
   The first line of code in which we are going to write is provider. We need to tell terraform which cloud provider we are going to connect .e.g - AWS, Google, or        Azure

   As this article is focused on AWS, so we are going to mention AWS as our provider.

   Here is the basic syntax for the provider
      
      resource "<PROVIDER>_<TYPE>" "<NAME>" {
      [CONFIG …]
      }
      
   1. "PROVIDER _ TYPE" - aws, google
   2. "NAME" - You can define your name
   
   This is how our main.tf will look like for AWS -
   
      provider "aws" {
      region     = "eu-central-1"
      access_key = "XXXXXXXXXXXXXXXXXXXX"
      secret_key = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
      } 
      
### 4.2 Resource - "aws_instance"
   Now after defining the provider, next we are going define is resource. 
   So what do you mean by resource?
   Resource - It is something that we are going to provision/start on AWS. Now for this article, we are going to provision EC2 instance on AWS.
   But before we provision the EC2 instance, we need to gather few points -
   1. ami = you need to tell Terraform which AMI(Amazon Machine Image) you are going to use. Is it going to be Ubuntu, CentOS or something else
   2. instance_type = Also based on your need you have to choose the instance_type and it can be t4g.nano, t4g.micro, t4g.small etc.

### 4.3 How to find ami(Amazon Machine Image)
   
   1. To find the correct ami you need to Goto: EC2
   
   ![image](https://user-images.githubusercontent.com/87687468/190343196-051e752a-a61d-4a6b-80d6-25369f41e97c.png)
   
   2. In the left Navigation you will find Images -> AMIs
   
   ![image](https://user-images.githubusercontent.com/87687468/190343512-54fb7a3c-d048-4c23-bb66-0a0ebfb0fa80.png)
   
   3. On the search menu click on public images and then apply filters as per your reuirment. e.g. architecture=arm64, platform=ubuntu.
      copy the AMI ID from the search result.
   
   ![image](https://user-images.githubusercontent.com/87687468/190345166-846344fe-09b8-4ab8-96b0-907b67fd0abd.png)

### 4.4 How to find correct instance_type
   
   We can find the correct ìnstance_type` by visiting [this page](https://aws.amazon.com/ec2/instance-types/).

   Since I am looking for a very basic instance_type not production level instance, so I choose t4g.nano
   Here is the aws_instance configuration -
   
      resource "aws_instance" "ec2_example" {
      ami = "ami-02a92e06fd643c11b"  
      instance_type = "t4g.nano" 
      key_name= "aws_key"
      vpc_security_group_ids = [aws_security_group.main.id]
      }

#### Here is our complete main.tf -
    
         provider "aws" {
            region     = "us-east-2"
            access_key = "Axxxxxxxxxxxxxxxxxxxx"
            secret_key = "JzZKiCia2vjbq4zGGGxxxxxxxxxxxxxxxxxxxxxx"
         }

         resource "aws_instance" "ec2_example" {
            ami = "ami-02a92e06fd643c11b"  
            instance_type = "t4g.nano" 
            key_name= "aws_key"
            vpc_security_group_ids = [aws_security_group.main.id]

            provisioner "remote-exec" {
               inline = [
               "touch hello.txt",
               "echo helloworld remote provisioner >> hello.txt",
               ]
            }
             provisioner "local-exec" {
               command = "echo ${self.private_ip} >> private_ips.txt && echo ${self.public_ip} >> public_ips.txt && echo ${self.public_dns} >> public_ips.txt"
               }
            connection {
               type        = "ssh"
               host        = self.public_ip
               user        = "ubuntu"
               private_key = file("/home/ubuntu/aws/aws_key")
               timeout     = "4m"
            }
         }

         resource "aws_security_group" "main" {
            egress = [
              {
               cidr_blocks      = [ "0.0.0.0/0", ]
               description      = ""
               from_port        = 0
               ipv6_cidr_blocks = []
               prefix_list_ids  = []
               protocol         = "-1"
               security_groups  = []
               self             = false
               to_port          = 0
              }
            ]
             ingress = [
              {
                cidr_blocks      = [ "0.0.0.0/0", ]
                description      = ""
                from_port        = 22
                ipv6_cidr_blocks = []
                prefix_list_ids  = []
                protocol         = "tcp"
                security_groups  = []
                self             = false
                to_port          = 22
              }
            ]
         }

         resource "aws_key_pair" "deployer" {
            key_name   = "aws_key"
            public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDbvRN/gvQBhFe+dE8p3Q865T/xTKgjqTjj56p1IIKbq8SDyOybE8ia0rMPcBLAKds+wjePIYpTtRxT9UsUbZJTgF+SGSG2dC6+ohCQpi6F3xM7ryL9fy3BNCT5aPrwbR862jcOIfv     7R1xVfH8OS0WZa8DpVy5kTeutsuH5suehdngba4KhYLTzIdhM7UKJvNoUMRBaxAqIAThqH9Vt/iR1WpXgazoPw6dyPssa7ye6tUPRipmPTZukfpxcPlsqytXWlXm7R89xAY9OXkdPPVsrQdkdfhnY8aFb9XaZP8cm7EOVRdxMsA1DyWMVZOTjhBwCHfEIGoePAS3jFMqQjGWQd rahul@rahul-HP-ZBook-15-G2"
         }                  

#### NOTE : "Key_name" & "Public_key" should be replaced with original value. 
       
### 5. Terraform commands
    
   Now we have completed all the pre-requisites for provisioning our first ec2 instance on the AWS.
    
### 5.1 terraform init
    
   The first command which we are going to run is -
    
   ![image](https://user-images.githubusercontent.com/87687468/190346484-4aa27809-c1ab-44ed-989b-9dcb232345f3.png)
    
   ![image](https://user-images.githubusercontent.com/87687468/190346590-e5be6def-5d6b-470a-a0cb-1057a1334cd7.png)
      
   The terraform init command is responsible for downloading all the dependencies which are required for the provider AWS.
   Once you issue the terraform init command it will download all the provider's dependencies on your local machine.

### 5.2 terraform plan
   
   This command will help you to understand how many resources you are gonna add or delete.

   Here is the command -

   ![image](https://user-images.githubusercontent.com/87687468/190347066-fa9cd09e-b3f0-44f1-9621-043bbc4b972d.png)
   
### 5.3 terraform apply
   This command will do some real stuff on AWS. Once you will issue this command, it will be going to connect to AWS and then finally going to provision AWS instance.

   Here is the command -   
   
   ![image](https://user-images.githubusercontent.com/87687468/190377240-b37c607f-38b3-415d-836e-ae84abfd627b.png)

   As you can see the log output has created t4g.nano instance.
   
### 5.4 Verify the EC2 setup
   Let's verify the setup by going back to AWS console.

   Goto -> EC2 -> instances you should see 1 instance running.   
   
   ![image](https://user-images.githubusercontent.com/87687468/192154191-7c0c97c6-4119-4395-bd8a-2873835e2f73.png)

   You can also see the Tag name - Terraform EC2 which we mentioned in the terraform script.
   
### 5.5 Use private key 'aws_key' to SSH into EC2 instance
In the previous step, we have started the EC2 instance, now we need to connect to EC2 instance using the private key.

You can find the connect command from the aws console -

![image](https://user-images.githubusercontent.com/87687468/190621116-0e9fb285-960f-437d-bfc0-77352349372c.png)   
   
### 5.6 terraform destroy
   
   Now we have seen how to write your terraform script and how to provision your EC2 instance.

   Let see how to remove or delete everything from AWS.

   We are going to use the command -
   
   ![image](https://user-images.githubusercontent.com/87687468/190385964-c54095c3-88be-4eae-a131-0fb41cad24cb.png)

   It will remove all the running EC2 Instances.
