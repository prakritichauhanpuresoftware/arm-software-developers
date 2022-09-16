## What is Graviton
"Graviton is an Arm based process designed by Annapurna labs for AWS. More information can be found here"
The "here" text, should be a link to this page:
https://aws.amazon.com/pm/ec2-graviton/

## How to deploy Graviton based EC2 instances via GUI

Log-in to your aws account. When you Go to > EC2 dashboard. Here, In case we have no instances running (0 Running Instances), we need to Go to > instances and select Launch Instances.

   ![image](https://user-images.githubusercontent.com/87687468/189866780-e67c8a99-e5f2-445f-938c-a672cd926c4a.png)
   
## Step-1
### Name and Tags
   Give a proper name to server
    
   ![image](https://user-images.githubusercontent.com/87687468/189888083-b5b241cf-ed8f-4cb6-b414-3f7d478c9dc8.png)

## Step-2
### Choose an Amazon Machine Image (Ami)
   Go to > Ubuntu Server 22.04 LTS (HVM), SSD Volume Type, ami-0ff596d41505819fd, from Ubuntu.  
   Select and use > 64-bit (Arm) version.
   
   ![image](https://user-images.githubusercontent.com/87687468/189873264-568cb285-bf1f-40bc-9a84-c94407b65d0d.png)

## Step-3
### Choose an Instance Type
   Here we can choose the any instance type from various types availabe in drop down list as per our requirment. For Practice, Use a t4g.micro instance (low to            moderate). Go to> Next
   
   ![image](https://user-images.githubusercontent.com/87687468/189874028-ad3955ec-817d-4cac-a3f2-82335a256500.png)
 
 
## Step-4
### Key pair (login)
   We can use a key pair to securely connect to our instance. Either we can choose from existing key pair or we can create a new one. It is generally preferred that      instead of using the same key pair for all servers, you should create a new one for certain groups of servers. Though, you might want to keep one key pair per          account where you need to have only 3-4 servers per account which might be the case for small webapps.
   
   ![image](https://user-images.githubusercontent.com/87687468/189890580-0b647d1e-baad-4597-95ad-7fcad81e9324.png)

   Now, for creating a new Key pair Go to > “ Create a new pair” and then name it.  e.g. ‘demoserver’. then select type and Private key file format and press ""create    key pair"

   ![image](https://user-images.githubusercontent.com/87687468/189891219-ac02d5df-d247-4adb-8e3d-03c0212b9356.png)


   Download the newly created Key Pair. Once, it is downloaded, we can select the same from our dropdown list.
   
   ![image](https://user-images.githubusercontent.com/87687468/189892157-580783ad-f387-4f6e-83f6-793661078bbc.png)

## Step-4
### Network setting
   Let it be as default VPC and the default Subnet.

### Configure Security Group
   Here either we can choose an existing security group for our security configuration or we can crerate new security group as per our requirment.
   
   ![image](https://user-images.githubusercontent.com/87687468/189876379-1d9118c8-a9a6-4e6d-892a-e37443d37546.png)
   
## Step-5 
### Configure Storage
   By default, EC2 comes with an 8 GiB disk size but 8GiB is not sufficient for most of the server scenarios as you want to have some room for things like log files      and backups. So we need to choose a provision for 25 GiB disk space in general.
   Always use General Purpose SSD (GP2) unless you have a reason to choose a ‘Magnetic’ Disk. Provisional IOPS SSD (IO1) is a specialized type of disk which is very      expensive and should be used only for high performance database requirements.
   
   
   ![image](https://user-images.githubusercontent.com/87687468/189878035-87d9721f-c58e-4ce7-800b-093d4d3e59ce.png)
   
   You should use ‘Delete On Termination’ always. This way it will be ensured that this volume would be deleted once the server is deleted.
   
 ## Step-6
 ### Review And Launch
   check the details in summary box and press "launch".

   ![image](https://user-images.githubusercontent.com/87687468/189878839-3ef022f7-2be7-458a-b0ce-20cf5ee0bcaa.png)

 ## Step-7
 ### SSH to launched instance
   Once the server get launched we can login using SSH. For that, we would require to access the key pair (Pem file) which we have Downloaded. if we check the            permissions of .pem file, we might see that the permission for the Pem file is-
   
   ```
   rw-r--r--
   ```

   This means that other people can also read this Pem file. This is not allowed for SSH Pem files. So, we will have to change the permission for this Pem file to 400.
   
   ```
   $ chmod 400 demoserver.pem
   ```
   
   Now, only the current user can read this Pem file and these are permission that a Pem file expects.
   Select the Instance by checking the box of that perticular server and GO TO >> connect
   
   ![image](https://user-images.githubusercontent.com/87687468/190094594-19ebf2dc-de3f-449d-bbb1-30089346ceb3.png)
   
   Here in SSH Client, u can find the details to ssh that instance.
   
   ![image](https://user-images.githubusercontent.com/87687468/190095052-41851f3d-61db-486f-9c00-2f504587bdcc.png)
   
   Since, I am in the same directory where the Pem file is present. I do not need the full path for the pem file.
   Use following command to ssh an instance:
   
   ```
   ssh -i "demoserver.pem" ubuntu@<Public IP/DNS address>
   ```


## How to deploy Graviton based EC2 instances with Terraform
   This guide will help you to Create your first AWS EC2 using terraform.
   When it comes to IAC(Infrastructure As Code) Terraform is always the first choice of DevOps although there are many alternatives available in the market such as        Ansible, Chef, Puppet, SaltStack, CloudFormation but due the fact that -
   
   1. Terraform is really easy install
   2. Terraform has very good API documentation
   3. It is widely adopted in the DevOps community
   4. Great support for a popular cloud service provider such as Google Cloud Platform, AWS.

### Prerequisite
   The only prerequisite is - You must [install Terraform](https://www.terraform.io/cli/install/apt) before jumping to AWS instance Setup.
   
### Table of Content
   1. Setup AWS Account
   2. Generate Access keys (access key ID and secret access key)
   3. Create your first Terraform infrastructure (main.tf)
   4. terraform commands

### 1. Setup AWS Account
   As our aim of this article to setup an AWS EC2 instance the first step would be to create an AWS account.
   If you are a beginner and want to learn the Terraform then AWS provides you free tier - 12 months or 750 Hours/month, where you can experiment.

### 1.1 Sign up for AWS account
   Goto https://aws.amazon.com/ and click on Complete sign-up
      
   ![image](https://user-images.githubusercontent.com/87687468/190132437-19f68942-53b5-4837-9706-31a777d0f930.png)
      
   You need to choose AMAZON EC2 Free Tier. This tier is sufficient enough for learning purposes.
      
   ![image](https://user-images.githubusercontent.com/87687468/190132636-1312ee45-f68d-46d2-bddb-c4285816ca1a.png)

   Amazon will ask for your Credit card number to complete the sign-up process, AWS will debit around 1$ so that they can verify your card details. Amazon will           refund the amount after the authorization.

### 1.2 After Signup LogIn as ROOT user
   
   After signup, you need to log in as a ROOT user for your AWS account.
      
   ![image](https://user-images.githubusercontent.com/87687468/190132990-5fe0f4b9-5808-4bff-a321-b1d72b9bc2d5.png)
      
   Once you login into your AWS account you should see below dashboard
      
   ![image](https://user-images.githubusercontent.com/87687468/190133678-18fe9da6-e7f5-4e78-aeb2-35386bbb17cd.png)
      
      
### 2.Generate Access keys (access key ID and secret access key)
   
   Terraform installed on your Desktop/Laptop needs to communicate with AWS and to make this communication terraform needs to be authenticated.
   For authentication, we need to generate Access Keys (access key ID and secret access key). These access keys can be used for making - programmatic calls to AWS from    the AWS CLI, Tools for PowerShell, AWS SDKs, or direct AWS API calls.
   
   1. Goto My Security Credentials
   
   ![image](https://user-images.githubusercontent.com/87687468/190137370-87b8ca2a-0b38-4732-80fc-3ea70c72e431.png)

   2. On Your Security Credentials page click on create access keys (access key ID and secret access key)
   
   ![image](https://user-images.githubusercontent.com/87687468/190137925-c725359a-cdab-468f-8195-8cce9c1be0ae.png)
   
   3. Copy the Access Key ID and Secret Access Key 

   ![image](https://user-images.githubusercontent.com/87687468/190138349-7cc0007c-def1-48b7-ad1e-4ee5b97f4b90.png)

### 3. Create your first Terraform infrastructure (main.tf)
   Before we start writing terraform script, the first thing to learn over here is - "You need to save your configuration with .tf extension". We will start by            creating an empty main.tf file.
   
###   3.1 Provider
   The first line of code in which we are going to write is provider. We need to tell terraform which cloud provider we are going to connect .e.g - AWS, Google, or        Azure

   As this article is focused on AWS, so we are going to mention AWS as our provider.

   Here is the basic syntax for the provider
      
      resource "<PROVIDER>_<TYPE>" "<NAME>" {
      [CONFIG …]
      }
      
   1. "PROVIDER _ TYPE" - aws, google
   2. "NAME" - You can define your name
   
   This is how our final main.tf will look like for AWS -
   
      provider "aws" {
      region     = "eu-central-1"
      access_key = "XXXXXXXXXXXXXXXXXXXX"
      secret_key = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
      } 
      
###   3.2 resource - "aws_instance"
   Now after defining the provider, next we are going define is resource. 
   So what do you mean by resource?
   Resource - It is something that we are going to provision/start on AWS. Now for this article, we are going to provision EC2 instance on AWS.
   But before we provision the EC2 instance, we need to gather few points -
   1. ami = you need to tell Terraform which AMI(Amazon Machine Image) you are going to use. Is it going to be Ubuntu, CentOS or something else
   2. instance_type = Also based on your need you have to choose the instance_type and it can be t2.nano, t2.micro, t2. small etc.

###   3.3 How to find ami(Amazon Machine Image)
   
   1. To find the correct ami you need to Goto: EC2
   
   ![image](https://user-images.githubusercontent.com/87687468/190343196-051e752a-a61d-4a6b-80d6-25369f41e97c.png)
   
   2. In the left Navigation you will find Images -> AMIs
   
   ![image](https://user-images.githubusercontent.com/87687468/190343512-54fb7a3c-d048-4c23-bb66-0a0ebfb0fa80.png)
   
   3. On the search menu click on public images and then apply filters as per your reuirment. e.g. architecture=arm64, platform=ubuntu.
      copy the AMI ID from the search result.
   
   ![image](https://user-images.githubusercontent.com/87687468/190345166-846344fe-09b8-4ab8-96b0-907b67fd0abd.png)

###   3.4 How to find correct instance_type
   
   We can find the correct ìnstance_type` by visiting [this page](https://aws.amazon.com/ec2/instance-types/).

   Since I am looking for a very basic instance_type not production level instance, so I choose t2.micro
   Here is the aws_instance configuration -
   
      resource "aws_instance" "ec2_example" {
      ami = "ami-0767046d1677be5a0"  
      instance_type = "t2.micro" 
      tags = {
         Name = "Terraform EC2"
         }
      }
    
###   4. terraform commands
    
   now we have completed all the pre-requisites for provisioning our first ec2 instance on the AWS.
    
###   4.1 terraform init
    
   The first command which we are going to run is -
    
   ![image](https://user-images.githubusercontent.com/87687468/190346484-4aa27809-c1ab-44ed-989b-9dcb232345f3.png)
    
   ![image](https://user-images.githubusercontent.com/87687468/190346590-e5be6def-5d6b-470a-a0cb-1057a1334cd7.png)
      
   The terraform init command is responsible for downloading all the dependencies which are required for the provider AWS.
   Once you issue the terraform init command it will download all the provider's dependencies on your local machine.

###   4.2 terraform plan
   
   This command will help you to understand how many resources you are gonna add or delete.

   Here is the command -

   ![image](https://user-images.githubusercontent.com/87687468/190347066-fa9cd09e-b3f0-44f1-9621-043bbc4b972d.png)
   
###   4.3 terraform apply
   This command will do some real stuff on AWS. Once you will issue this command, it will be going to connect to AWS and then finally going to provision AWS instance.

   Here is the command -   
   
   ![image](https://user-images.githubusercontent.com/87687468/190377240-b37c607f-38b3-415d-836e-ae84abfd627b.png)

   As you can see the log output has created t2.micro instance.
   
### 4.4 Verify the EC2 setup
   Let's verify the setup by going back to AWS console.

   Goto -> EC2 -> instances you should see 1 instance running.   
   
   ![image](https://user-images.githubusercontent.com/87687468/190378211-484df7ca-87d3-4097-b2e0-635e0b34bbbf.png)

   You can also see the Tag name - Terraform EC2 which we mentioned in the terraform script.
   
### 4.4 terraform destroy
   
   Now we have seen how to write your terraform script and how to provision your EC2 instance.

   Let see how to remove or delete everything from AWS.

   We are going to use the command -
   
   ![image](https://user-images.githubusercontent.com/87687468/190385964-c54095c3-88be-4eae-a131-0fb41cad24cb.png)

   It will remove all the running EC2 Instances.




   
   


