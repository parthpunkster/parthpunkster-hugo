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

- Choosing AMI: select either **Amazon Linux 2 AMI (HVM), SSD Volume Type** or **Amazon Linux AMI 2018.03.0 (HVM), SSD Volume Type** (Preferred one)
  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image1.png">
  </figure>

- Choosing Instance type, choose **t2.micro (Free tier eligible)** and then click on **Next : Configure Instance Details** (**Note** - Do not click on Review and Launch)
  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image2.png">
  </figure>
Keep clicking on **Next** until you reach **Configure Security group**. (We will be following the default configuration for the rest and one can change it if required in future)

- Under the Configure Security group, either create a new security group or select an existing one. 
If creating a new security group then add the port 22 and port 8888 rules with source set to My IP.
If selecting an existing one then make sure there are port 22 and port 8888 rules.
Click on **Review and Launch**.
  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image3.png">
  </figure>

- Click on **launch**, At this point, you should be prompted with some security key options. You can use an existing key or download a new one.
  Let’s assume you don’t have one yet. The PEM file is a key that AWS will check when you try to access (or SSH) into your EC2 instanc from
  your local computer’s terminal.<br/><br/>Select the option to create a new one and give it an easy to remember name (all lower case with no spaces for ease of typing too). <br/>Download the PEM file and put it in an easy to reach location like your home folder. <br/>Now you can finally launch your EC2 instance by selecting Launch Instance.
  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image4.png">
  </figure>
Click on the **instance Id** to navigate to instance which is launched
  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image5.png">
  </figure>

- Select the instance which is launched and under the Description navigate to Security Group and click on **Default -> Inbound -> Edit**.
  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image6.png">
  </figure>
Then in source select **My IP** for **TCP and Port 8888** and also for **TCP and port 22**.
  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image7.png">
  </figure>

Navigate back to instances and to the newly launched instance.

## Step 3) SSH into the instance (Connect the device terminal to the AWS terminal)
Click on **Connect** and it will open up a dialogue box from that copy the last line 
```
ssh -i "tutorialexample.pem" ec2-user@ec2-54-144-47-199.compute-1.amazonaws.com
```

This assumes your PEM file is in the same directly as your present working directory (type “pwd” into your terminal if you’re unsure what your present working directory is). Also you’ll need to type in your own user@ec2-instance address. If you’ve been following my steps so far the user is “ec2-user” (this is the default) and the address after “@” is your instance’s Public DNS (IPv4) address which you can view by selecting your instance (see screenshot below). If you get an error that your PEM file is not publicly viewable, you made need to execute this command:
```
chmod 400 tutorialexample.pem
```
  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image8.png">
  </figure>

Once you execute the SSH command, you’ll be prompted with a yes/no question. Type yes and you should be SSH-ed into your instance! (see screenshot below).
  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image9.png">
  </figure>

Congratulation you have now successfully create a Linux AWS EC2 instance.
Now  run the following command to update the OS libraries
```
sudo yum update
```

## Step 4) Download and install Anaconda and Jupyter Notebook 
- Run the following command to download anaconda3, change the name according to the latest anaconda version available.
```
wget https://repo.continuum.io/archive/Anaconda3-5.3.0-Linux-x86_64.sh
```

- Run the following command to install the downloaded Anaconda3
```
bash Anaconda3-5.3.0-Linux-x86_64.sh
```
After this there will be a bigger legal terms and condition text, keep **pressing enter**, until it asks you to **enter yes or no** for accepting the terms and condition, type **Yes and press enter**.<br/>Now **press enter** to install anaconda to **default location(Preferred)** or **type the location** where you want to install it.<br/>Now it will ask Do you wish the installer to initialize Anaconda3 in your **/home/ec2-user/.bashrc ? [yes|no]**<br/>Enter **Yes**<br/>Wait...Wait...Wait for installation to finish.<br/>If you typed **yes** then goto **Step 5)**

- You will have to manually enter the path to **.bashrc** file.
Run the following commands:
```
Vim .bashrc
```
Press **i to edit** the file,
Paste the following path in **.bashrc** file **export PATH="/home/ec2-user/anaconda3/bin:$PATH"**
Press **esc** and then type **:wq** to save it
Then type
```
source .bashrc
```

## Step 5) Setting Anaconda3 as your default Python environment.
Currently the default Python environment is set to python 2.7, but we will shift to Anaconda3 for python 3 environment.
Use the following commands:
```
which python /usr/bin/python
```
Then type
```
source .bashrc
```

  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image10.png">
  </figure>

Now if you type 
```
python
```
it will show the new version of Anaconda and Python 3 version
**Press Ctrl+d or type quit()** to get out of the python REPL

## Step 6) Create Password for jupyter notebook
Since the notebook is on cloud and can be easily accessed by others and due to security concerns we will create a password for the jupyternotebook.
To create the password run the following commands:
```
Ipython
```
Then type
```
from IPython.lib import passwd
```
Then type
```
passwd()
```
Now you will be prompted to type and verify the password. After that an **SHA version** is shown save that somewhere as we will require the SHA later on
It will look something like this
  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image11.png">
  </figure>

## Step 7) Configure Jupyter  server to access the notebooks from your local computer via internet browser.
First we’ll create a default config file by just typing:
```
jupyter notebook --generate-config
```
Next, we’ll need to generate SSL certificates so our browser will trust our Jupyter Notebooks server, just type:
```
mkdir certs
```
Then goto the certs directory by typing
```
cd certs
```
then create your PEM file (this is separate from the PEM file on your local computer which we downloaded from AWS) by typing:
```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem
```
This certificate is good for one year. You will be prompted to enter in some personal info for your certificate, but you can type in whatever you want. When you’re done, it should look something like this:
  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image12.png">
  </figure>
Type the following command to go to home directory
```
cd
```

## Step 8) Edit your jupyter configuration file
Follow the below commands :
```
vim .jupyter/jupyter_notebook_config.py
```
**Press i**
Paste the code below in the above file
```
c = get_config()

# Kernel config
c.IPKernelApp.pylab = 'inline'  # if you want plotting support always in your notebook

# Notebook config
c.NotebookApp.certfile = u'/home/ec2-user/certs/mycert.pem' #location of your certificate file
c.NotebookApp.ip = '*'
c.NotebookApp.open_browser = False  #so that the ipython notebook does not opens up a browser by default
c.NotebookApp.password = u'sha1:262....your hash here.........65f'  #edit this with the SHA hash that you generated after typing in Step 9
# This is the port we opened in Step 3.
c.NotebookApp.port = 8888
```
**Insert your own SHA in NotebookApp.password**
  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image13.png">
  </figure>

## Step 9) Create a folder for your notebooks by typing the following commands
```
mkdir Notebooks
```
And then goto the created folder by typing
```
cd Notebooks
```

## Step 10) open jupyter notebook
Type 
```
jupyter notebook
```
You should see something like this now:
  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image14.png">
  </figure>

## Step 11) Accessing jupyter notebook from web broswer
Type the following url:
```
https://(Your AWS machines public DNS over here):8888/
```
**E.g : https://ec2-54-144-47-199.compute-1.amazonaws.com:8888/**
Then click on **Proceed** to Show Advanced Security and then click on the **link** at the bottom.



Press ESC and then type :wq
It should look something like this
  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image15.png">
  </figure>

Next you’ll see the password protection screen for Jupyter. Just type in the actual password you chose in Step 9 (not the SHA hash)
  <figure>
      <img style="width:100%;" src="/img/post/installing-anaconda-jupyternotebook-java-pyspark-in-awsec2-linux/image16.png">
  </figure>

Click **Log In** and you’re ready to create and run your own notebooks on Jupyter!

Woohooooo you have successfully installed and launched jupyter notebook

Now go to your system terminal and **press ctrl+c** to close the jupyter kernel properly 
Then type the following to go to your home directory 
``
cd
`` 

## Step 11) Install Java
The java already installed is java 1.7, we need to upgrade to the latest available version java we will install java 1.8.0, but you can upgrade to more latest version just by changing the version number in the commands below.
To Install Java Runtime 1.8
```
sudo yum install java-1.8.0
```
To install java compiler and other developer tools
```
sudo yum install java-1.8.0-openjdk-devel
```
Then use the alternatives command to make Java 1.8 the default.
```
sudo /usr/sbin/alternatives --config java
sudo /usr/sbin/alternatives --config javac
```
Preferred is not to remove java 1.7
If you prefer you can remove Java 1.7 with
```
sudo yum remove java-1.7.0-openjdk
```

To recheck whether it has been updated to java 1.8 and above, type the following command
```
java -version
``` 

## Step 12) Connecting pip to Anaconda installation
We need to make sure that pip install is connected to our Anaconda installation of Python instead of Ubuntu’s default. In the console we will export the path for pip by typing the following commands:
```
vim .bashrc
```
**Press i**
Paste this in **.bashrc** file : 
```
export PATH=$PATH:$HOME/anaconda3/bin
```
Press ESC and then type 
```
:wq**
```
Type 
```
source .bashrc
```

Now we will use conda to install pip by typing :
```
$ conda install pip
```
Confirm that the correct pip is being used with:
```
$ which pip
```

## Step 13) Installing py4j in conda environment using the pip from the conda enviroment
```
$ pip install py4j
```

## Step 14) Install Spark and Hadoop
To download spark and hadoop we will type the following command with appropriate and latest version of spark available, we will be using spark-2.3.2-bin-hadoop2.7
```
$ wget http://archive.apache.org/dist/spark/spark-2.3.2/spark-2.3.2-bin-hadoop2.7.tgz
```

Installing the downloaded spark and hadoop by typing
```
$ sudo tar -zxvf spark-2.3.2-bin-hadoop2.7.tgz
```

## Step 15) Tell Python where to find Spark
We need to set paths for spark so that python can find the packages.
```
$ vim .bashrc
```
**Press i**
Paste the following lines based on the path of spark in your computer
```
export SPARK_HOME='/home/ubuntu/spark-2.3.2-bin-hadoop2.7'
export PATH=$SPARK_HOME:$PATH
export PYTHONPATH=$SPARK_HOME/python:$PYTHONPATH
```

## Step 16) Run jupyter notebook and check whether spark is working or not.
To launch jupyter notebook type :
```
cd Notebooks
```
Now folllow step 10 and 11.
Create a new python 3 notebook and paste the following code and run it:
```
import findspark
findspark.init()
Import pyspark
```

If it runs without error then woohhoooooo you have successfully launched an aws ec2 machine using Linux OS and install jupyter notebook using anaconda version and also installed spark.
**Now you can run your ML codes.**







  

