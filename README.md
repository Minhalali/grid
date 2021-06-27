# Grid task

**Project Title:**
Grid Singularity Tasks

**Motivation:**
To complete the Tasks for the position of Junior DevOps Engineer at Grid Singluarity

**Build Status:**
Finished: SUCCESS

**Screenshot**
![image](https://user-images.githubusercontent.com/46167355/123520502-7e8f7200-d6ca-11eb-9edf-674939a658c8.png)

**Technology Used:**
Terraform, AWS EC2, Docker, Jenkins, Nginx 

**Features:**
This automation builds and deploy a container on a EC2 Instance using CI/CD pipeline using Jenkins.

**Installation:**
We first have to create an automation using Terraform to provision one Linux VM in AWS which will capable to run docker containers.
Install Terraform on the master server from where we will provision a linux instance on AWS. Once done we need to configure the main.tf file. 

![image](https://user-images.githubusercontent.com/46167355/123520682-3de42880-d6cb-11eb-9958-29283f5651f1.png)

Upon doing:

-> terraform init
-> terraform plan
-> terraform apply 

We will be able to see a new AWS EC2 instance in our dashboard:

![image](https://user-images.githubusercontent.com/46167355/123520737-8bf92c00-d6cb-11eb-9e61-85667351b15f.png)

Now we have to run docker containers on the newly created EC2 instance with the port 80 exposed to the Internet. In order to do so we will install Docker on our VM. I have install Docker using apt for the ubuntu server. Once docker is installed verify:

![image](https://user-images.githubusercontent.com/46167355/123520821-2a858d00-d6cc-11eb-8888-f72cfa706d0e.png)

Now we have to create a dockerfile for nginx so it runs on PORT 80. Below is the code for the dockerfile to install and run nginx on port 80


FROM ubuntu:latest

LABEL maintainer="m.minhalali@hotmail.com"

RUN apt-get update && apt-get upgrade -y

RUN apt-get install nginx -y

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]


Once done build and run:

-> docker build -t dockerfile .
-> docker run -p 80:80 -d dockerfile

**Output:**
![image](https://user-images.githubusercontent.com/46167355/123520954-08403f00-d6cd-11eb-9c16-0497b7fab4fa.png)

Next we will verify from the browser that nginx is running. 
-> http://18.117.168.64

![image](https://user-images.githubusercontent.com/46167355/123520984-332a9300-d6cd-11eb-9c1a-a62001858c89.png)

**CI/CD Automation:**
Next we have to create CI/CD Pipeline to build and deploy the container in our EC2 instance using Jenkins. We will create two jobs inside our pipeline, One will be use to build the container and the code and second to deploy the container. We will first install Jenkins on our server. I have already done that using apt. 

![image](https://user-images.githubusercontent.com/46167355/123521489-5c005780-d6d0-11eb-92db-dd271995871d.png)


Next jenkins will run by default on the port 8080.

-> http://18.117.168.64:8080

![image](https://user-images.githubusercontent.com/46167355/123521111-0aef6400-d6ce-11eb-9d6a-7f690378ac66.png)

Next for this project we will add two jobs as freestyle projects. Add our git repo in the tasks and configure the executable shell code. Also POST-BUILD Actions will be set on Job 1 that will point towards Job 2. 

Job 1 (Grid):

![image](https://user-images.githubusercontent.com/46167355/123521247-e8aa1600-d6ce-11eb-810b-ec04f7f85bdb.png)

Job 2 (gridjob2):

![image](https://user-images.githubusercontent.com/46167355/123521260-feb7d680-d6ce-11eb-886e-21741abf820b.png)

**How to use:**
In the last step we create a new list to organize our work and Pipeline on Jenkins. 

![image](https://user-images.githubusercontent.com/46167355/123521300-3c1c6400-d6cf-11eb-9b2b-a607abdda1d2.png)

All we need to do now is to build the job.

![image](https://user-images.githubusercontent.com/46167355/123521337-7be34b80-d6cf-11eb-9ef1-7a61b6c26186.png)


![image](https://user-images.githubusercontent.com/46167355/123521359-9caba100-d6cf-11eb-97e6-e25e4328d079.png)

As we can see above a new container has been deployed on our EC2 instance. 

![image](https://user-images.githubusercontent.com/46167355/123521512-7e927080-d6d0-11eb-9965-6fed6ade32b3.png)


**Notes:** 
- Credentials are to be used as Enviorment Variables so they are not exposed. For example: IAM User Access key, Secret key.
- Sample application Dockerfile code has been used. 


