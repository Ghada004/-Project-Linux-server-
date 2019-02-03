# Udacity - Linux Server Configuration



# IP & Hostname

 IP Address: 18.219.25.66
 
 Hostname: ec2-18-219-25-66.us-east-2.compute.amazonaws.com
 
 
# SSH (port 2200), HTTP (port 80), and NTP (port 123):
## ssh -i ~/.ssh/udacity.rsa grader@18.219.25.66 -p 2200


# Project Overview

I took a baseline installation of a Linux server and prepare it to host your web applications. You will secure your server from a number of attack vectors, install and configure a database server, and deploy one of your existing web applications onto it.

## How you can complete this project?

To complete this project, you'll need a Linux server instance. and use Amazon Lightsail for this. 

## Linux Configuration :

1- On Mac you will want to store all of your SSH Keys in the .ssh folder which is located in a folder called .ssh at the root of your user directory. For example Macintosh HD/Users/[Your username]/.ssh/

2. If you cannot see that folder in the finder open the terminal and type: $ killall Finder then type $ defaults write com.apple.finder AppleShowAllFiles TRUE
Now move your downloaded .pem key into that folder.
To make our key secure type $ chmod 600 ~/.ssh/udacity.pem into the terminal.

3- From here we will log into the server as the user ubuntu with our key. From the terminal type $ ssh -i ~/.ssh/udacity.pem ubuntu@0.0.0.0 
(where 0.0.0.0 is you Public IP)

4- Once logged in you will see the command line change to root@[ip-your-private-ip]:$

5- Lets switch to the root user by typing sudo su -
As Udacity requires we need to create a user called grader.

6- From the command line type $ sudo adduser grader. It will ask for 2 passwords and then a few other fields which you can leave blank.

7- We must create a file to give the user grader superuser privileges. To do this type $ sudo nano /etc/sudoers.d/grader. 

8 - This will create a new file that will be the superuser configuration for grader. When nano opens type grader ALL=(ALL:ALL) ALL , 

9- to save the file hit Ctrl-X on your keyboard, type 'Y' to save, and return to save the filename.

10 - One of the first things you should always do when configuring a Linux server is updating it's package list, upgrading the current packages, and install new updates with these three commands:
```
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get dist-upgrade
```

11- We will also install a useful tool called Finger with the command ``` $ sudo apt-get install finger.```
This tool will allow us to see the users on this server.
Now we must create an SSH Key for our new user grader.

12- From a new terminal run the command: $ ssh-keygen -f ~/.ssh/udacity.rsa
In the same terminal we need to read and copy the public key using the command: ``` $ cat ~/.ssh/udacity.rsa.pub.```
Copy the key from the terminal.

13- Back in the server terminal locate the folder for the user grader, it should be /home/grader. Run the command $ cd /home/grader to move to the folder.

14- Create a directory called .ssh with the command $ mkdir .ssh

15- Create a file to store the public key with the command $ touch .ssh/authorized_keys
Edit that file using $ nano .ssh/authorized_keys
Now paste in the public key.

16- We must change the permissions of the file and its folder by running
```
$ sudo chmod 700 /home/grader/.ssh
$ sudo chmod 644 /home/grader/.ssh/authorized_keys 
```
Change the owner of the .ssh directory from root to grader by using the command ``` $ sudo chown -R grader:grader /home/grader/.ssh```

17- The last thing we need to do for the SSH configuration is restart its service with $ sudo service ssh restart
Disconnect from the server

18- Now we need to login with the grader account using ssh. From your local terminal type $ ssh -i ~/.ssh/udacity.rsa grader@0.0.0.0 
(where 0.0.0.0 is you Public IP)

19- You should now be logged into your server via SSH
Lets enforce key authentication from the ssh configuration file by editing $ sudo nano /etc/ssh/sshd_config. Find the line that says PasswordAuthentication and change it to no. Also find the line that says Port 22 and change it to Port 2200. Lastly change PermitRootLogin to no.

20- Restart ssh again: $ sudo service ssh restart
Disconnect from the server and try step "23." again BUT also adding -p 2200 at the end this time. You should be connected.
From here we need to configure the firewall using these commands:
```
$ sudo ufw allow 2200/tcp
$ sudo ufw allow 80/tcp
$ sudo ufw allow 123/udp
$ sudo ufw enable
```

21 Running $ sudo ufw status should show all of the allowed ports with the firewall configuration.
That pretty much wraps up the Linux configuration, now onto the app deployment.


## Amazon Lightsail Set Up
Go to the Amazon Lightsail website [https://aws.amazon.com/lightsail/]

# grader Password: 12345 

name: ghada


# MY udacity.rsa File

PRIVATE KEY.text file

## References:

https://github.com/mulligan121/Udacity-Linux-Configuration/blob/master/README.md

https://github.com/chuanqin3/udacity-linux-configuration

