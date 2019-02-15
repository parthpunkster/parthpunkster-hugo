+++
title = "Installing Anaconda, Jupyter Notebook, Java and PySpark in AWS EC2 Linux Instance"
date = 2019-02-14T23:45:56-08:00
draft = false
+++

This article is for installing anaconda, jupyter notebook java, pyspark on aws ec2 linux machine with the most basic configuration which is eligible in the free tier account for students.
The below configuration is for ubuntu but should work for other OS as well.
We assume that you already have an AWS account. If not then create one and then follow the below steps:

## Step 1) Sign-in into your aws account
- Goto [aws](https://aws.amazon.com/).
- click on **My Account -> AWS Management Console.**
- Sign-in page will appear, Sign-in into your account.

## Step 2) Launch EC2 instance with basic configuration
- Click on **Services** -> click on **EC2** under the Compute category, to launch an EC2 instance.
  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image0.png">
  </figure>

- Click on **Launch Instance** under the Create Instance section.

- Choosing AMI: select either Amazon Linux 2 AMI (HVM), SSD Volume Type or Amazon Linux AMI 2018.03.0 (HVM), SSD Volume Type (Preferred    one)
  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image1.png">
  </figure>

- Choosing Instance type, choose t2.micro (Free tier eligible) and then click on Next : Configure Instance Details (Do not click on Review and Launch)
  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image2.png">
  </figure>
Keep clicking on Next until you reach Configure Security group. (We will be following the default configuration for the rest and one can change it if required in future)

- Under the Configure Security group, either create a new security group or select an existing one. 
  If creating a new security group then add the port 22 and port 8888 rules with source set to My IP.
  If selecting an existing one then make sure there are port 22 and port 8888 rules.
  Click on Review and Launch.
  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image3.png">
  </figure>

- Click on launch, At this point, you should be prompted with some security key options. You can use an existing key or download a new one.
  Let’s assume you don’t have one yet. The PEM file is a key that AWS will check when you try to access (or SSH) into your EC2 instanc from
  your local computer’s terminal.
  
  Select the option to create a new one and give it an easy to remember name (all lower case with no spaces for ease of typing too). 
  Download the PEM file and put it in an easy to reach location like your home folder. 
  Now you can finally launch your EC2 instance by selecting Launch Instance.

