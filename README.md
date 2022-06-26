Deploying large python packages on AWS Lambda using EFS + Serverless API  using API Gateway

In this chapter, we will work a few of the many different services that aws provides and how it provides us with many different tools to be able to deploy our work.

What is lambda?

AWS Lambda is an AWS service that lets you run code without provisioning or managing servers. So it’s serverless. This means that the user is only interacting with the function as an entity, not with anything related to the server they are running. This simplifies some operations and costs as AWS only charges for the time the function uses.

However, there may be some restrictions in the working model of lambda, such as the time the code could be running, memory, and such. These constraints may enable us to use lambda with different solutions.

Now let's consider python libraries.

What can we do if we want to run libraries that do not come with the basic python installation, plus a library on memory that exceeds the limits of the Lambda?

- We can install these libraries on an EFS and connect the EFS to the Lambda function.

What is EFS?

EFS stands for Elastic File System and is a file system located inside a VPC (Virtual private cloud) that allows you to store or access data. Having the libraries in EFS and having the Lambda function we will use use these libraries. 

Let's do it step by step 

1. VPC (Virtual Private Cloud)

Everything happens in a VPC context. That's why we need to create a VPC (or use a prebuilt VPC) first.

Next, you choose to create a simple VPC with a public network and we can give it a name.

<img width="851" alt="Ekran Resmi 2022-06-25 17 16 33" src="https://user-images.githubusercontent.com/91700155/175808422-5e9e1de9-beb8-46c6-ad61-08ca4af2277d.png">
<img width="851" alt="Ekran Resmi 2022-06-25 17 16 47" src="https://user-images.githubusercontent.com/91700155/175808423-bf4241e7-dcaa-4209-9784-fe17faa03e03.png">

Then we have our newly created VPC.

2. Create Security Group

AWS is the arrangement of many interactions of different components to ensure security. For most of these interactions, we use what is called a "security group", which contains sets of rules that allow an entity to connect to the outside (outbound rules) or allow something to connect to it from outside the entity (inbound rules).

"Create security group" and then add an inbound rule for NFS. For this exercise, we’ll arrenged that.

<img width="851" alt="Ekran Resmi 2022-06-25 17 22 16" src="https://user-images.githubusercontent.com/91700155/175808488-2731fd09-bca7-4658-9760-08d18e154303.png">


3. Create and configure EFS

Go to console and look for EFS. Then click in "create file system" and then give a name and choose the VPC that the EFS would be connected to.

<img width="425" alt="Ekran Resmi 2022-06-25 17 23 27" src="https://user-images.githubusercontent.com/91700155/175808720-931262cf-ef5e-4225-a8cf-993d144ae4d4.png">

- Network 
Select the EFS that you created and click the network area:

<img width="1131" alt="Ekran Resmi 2022-06-25 17 23 53" src="https://user-images.githubusercontent.com/91700155/175808882-a3d11974-7941-4634-9969-720a88c71d25.png">

Then click a button that says "Manage". 
Now we will add security gorup that we created before in addition you can add default security group. 

<img width="1152" alt="Ekran Resmi 2022-06-25 17 28 54" src="https://user-images.githubusercontent.com/91700155/175808937-b67a01bf-fa94-4078-a98c-bd7b6b422d5f.png">

- Access Point
Select the EFS that you created and click on Access points. Then click on "Create access point".

<img width="851" alt="Ekran Resmi 2022-06-25 17 30 32" src="https://user-images.githubusercontent.com/91700155/175809044-15583814-36bf-49d4-bdc0-5c1f9d2b8e37.png">

The important part is the directory name at the end. This folder is where we'll have the libraries that the Lambda Function will be able to connect to.
The rest is optional. I filled in the optional parts as in the appendix.

<img width="567" alt="Ekran Resmi 2022-06-25 17 32 53" src="https://user-images.githubusercontent.com/91700155/175809163-340c8ba3-2676-4c00-9288-d6626f427d8e.png">
<img width="567" alt="Ekran Resmi 2022-06-25 17 33 01" src="https://user-images.githubusercontent.com/91700155/175809166-12b296cb-0bb1-41b4-9c79-497aca5a28fa.png">


4. Test that the EFS is working
Now we'll test that EFS works like a proper file system and that we can interact with it. Before we create a lambda function, we need to define the privileges of the IAM role we will use.

- IAM (Identity and access management) Role
You can access the IAM role via the AWS console. Create role and add to policies that we need.

<img width="425" alt="Ekran Resmi 2022-06-25 17 35 17" src="https://user-images.githubusercontent.com/91700155/175809282-d7face26-c099-444c-a830-b162bf89021d.png">


5. Create Lambda Function
Again, we will create Lambda over the AWS console. I used Python 3.8 for this work, but you could choose a different version or language.

<img width="851" alt="Ekran Resmi 2022-06-25 17 36 51" src="https://user-images.githubusercontent.com/91700155/175809422-86fa47cc-8657-48f3-a859-580bf37e88be.png">

- In the permission section, we proceed by selecting the IAM role we created.

<img width="1041" alt="Ekran Resmi 2022-06-25 17 36 58" src="https://user-images.githubusercontent.com/91700155/175809484-592f475f-f568-4c8b-a419-0e4704cbdb44.png">

- Lambda function has been created. Now, go inside Lambda functions click on "Configuration".

<img width="685" alt="Ekran Resmi 2022-06-25 17 39 39" src="https://user-images.githubusercontent.com/91700155/175809587-e5f92f74-b3f2-46a0-aa13-70538792999e.png">

- From here we select the VPC area Choose the subnet and add a VPC and Security Group we created earlier here. 

<img width="567" alt="Ekran Resmi 2022-06-25 17 40 58" src="https://user-images.githubusercontent.com/91700155/175809626-4370092b-2437-43a9-8971-aa7d341fed0b.png">

- From Configuration we click on "File System" area and add a EFS we created earlier here.

<img width="567" alt="Ekran Resmi 2022-06-25 17 42 12" src="https://user-images.githubusercontent.com/91700155/175809718-f5040313-dded-49ff-8f72-80a2b8733a40.png">

Here choose the EFS we created before, add the Access point and the path where we are going to mount the EFS.

6. Test connection between Lambda and EFS
Here we will test whether the connection between EFS and Lambda works with a small example.

Go to the "Code" part of your Lambda. Copy the following exapmle code.


import json
import os
import sys
sys.path.append('/mnt/access')
def lambda_handler(event, context):
   
    
    print('Before: ',os.listdir('/mnt/access'))
    
    with open('/mnt/access/somefile.txt','w') as file:
        file.write('hello world')
    
    
    print('After: ',os.listdir('/mnt/access'))
    


7. Create an EC2 instance

7. Connect EC2 instance

8. Lambda Function using libraries from EFS

9. Create API Gateway

10. API Gateway test example

