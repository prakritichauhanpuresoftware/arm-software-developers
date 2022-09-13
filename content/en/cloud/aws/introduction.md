## What is Graviton
"Graviton is an Arm based process designed by Annapurna labs for AWS. More information can be found here"
The "here" text, should be a link to this page:
https://aws.amazon.com/pm/ec2-graviton/

## How to deploy Graviton based EC2 instances via GUI
Please Follow below steps to Deploy Graviton based instances :
1. Loging to your aws account.
2. When you Go to > EC2 dashboard. Here, In case we have no instances running (0 Running Instances), we need Go to > instances and select Launch Instances.

![image](https://user-images.githubusercontent.com/87687468/189866780-e67c8a99-e5f2-445f-938c-a672cd926c4a.png)

3. Choose an Amazon Machine Image (Ami)
   Go to > Ubuntu Server 22.04 LTS (HVM), SSD Volume Type, ami-0ff596d41505819fd, from Ubuntu.  
   Select and use > 64-bit (Arm) version.
   
   ![image](https://user-images.githubusercontent.com/87687468/189873264-568cb285-bf1f-40bc-9a84-c94407b65d0d.png)

4. Choose an Instance Type
   For Practice, Use a t4g.micro instance (low to moderate). Go to> Next
   
   ![image](https://user-images.githubusercontent.com/87687468/189874028-ad3955ec-817d-4cac-a3f2-82335a256500.png)
   
5. Configure Instance Details
   Here number of instance is ‘1’. Let it be as default VPC and the default Subnet.

6. Configure Security Group
   either we can choose an existing security group for our security configuration or we can crerate new security group as per our requirment.
   
   ![image](https://user-images.githubusercontent.com/87687468/189876379-1d9118c8-a9a6-4e6d-892a-e37443d37546.png)
   
7. Add Storage
   By default, EC2 comes with an 8 GiB disk size but 8GiB is not sufficient for most of the server scenarios as you want to have some room for things like log files and backups. So we need to choose a provision for 25 GiB disk space in general.
   Always use General Purpose SSD (GP2) unless you have a reason to choose a ‘Magnetic’ Disk.
   
8. check the details in summary box and press launch.

   
 
  ![image](https://user-images.githubusercontent.com/87687468/189878839-3ef022f7-2be7-458a-b0ce-20cf5ee0bcaa.png)

   Provisional IOPS SSD (IO1) is a specialized type of disk which is very expensive and should be used only for high performance database requirements.
   
   ![image](https://user-images.githubusercontent.com/87687468/189878035-87d9721f-c58e-4ce7-800b-093d4d3e59ce.png)
   
   You should use ‘Delete On Termination’ always. This way it will be ensured that this volume would be deleted once the server is deleted.

