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

Select the EFS that you created and click the network area:

<img width="1131" alt="Ekran Resmi 2022-06-25 17 23 53" src="https://user-images.githubusercontent.com/91700155/175808882-a3d11974-7941-4634-9969-720a88c71d25.png">

Then click a button that says "Manage". 
Now we will add security gorup that we created before in addition you can add default security group. 

<img width="1152" alt="Ekran Resmi 2022-06-25 17 28 54" src="https://user-images.githubusercontent.com/91700155/175808937-b67a01bf-fa94-4078-a98c-bd7b6b422d5f.png">

4. Test that the EFS is working

5. Create Lambda Function

6. Create an EC2 instance

7. Connect EC2 instance

8. Lambda Function using libraries from EFS

9. Create API Gateway

10. API Gateway test example

