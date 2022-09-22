---
title: "Deploy Graviton based EC2 instances using GUI"
type: docs
weight: 2
hide_summary: true
description: >
    Learn how to Deploy Graviton based EC2 instances using GUI.
---


# How to deploy Graviton based EC2 instances via GUI

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
