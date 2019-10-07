# Project_3
# Linux Server Configuration Project
### Project Overview
>> A baseline installation of a Linux server and prepare it to host web applications. Learning how to secure your server from a number of attack vectors, install and configure a database server, and deploy one of your existing web applications onto it.
* IP Address: [3.8.127.40]
* SSH Port: [2200]
### What did I learn?
>> I have learnt how to access, secure, and perform the initial configuration of a bare-bones Linux server. You will then learn how to install and conzfigure a web and database server and actually host a web application.

This page explains how to secure and set up a Linux distribution on a virtual machine, install and configure a web and database server to host a web application. 
- The Linux distribution is [Ubuntu](https://www.ubuntu.com/download/server) 16.04 LTS.
- The virtual private server is [Amazon Lighsail](https://lightsail.aws.amazon.com/).
- The web application is my [Item Catalog project](https://github.com/omrkhoder/project_2.git) created earlier in this Nanodegree program.
- The database server is [PostgreSQL](https://www.postgresql.org/).

## Steps to Configure Linux server
##### 1. Start a new Ubuntu Linux server instance on Amazon Lightsail [Amazon Lighsail](https://lightsail.aws.amazon.com/):

##### 2. Update all currently installed packages:
```
$ sudo apt-get update
$ sudo apt-get upgrade
sudo apt-get dist-upgrade
```
Enable automatic security updates
```
sudo apt-get install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades
```
##### 3. Create a new user grader and Give him `sudo` access:
```
sudo adduser grader
sudo nano /etc/sudoers.d/grader 
```
Then add the following text `grader ALL=(ALL) ALL`

##### 4. Setup SSH keys:
`ssh-keygen`

```
sudo su - grader
mkdir .ssh
touch .ssh/authorized_keys 
sudo chmod 700 .ssh
sudo chmod 600 .ssh/authorized_keys 
nano .ssh/authorized_keys 
```
Then paste the contents of the public key created on the local machine

##### 5. Change the SSH port from 22 to 2200:

- Edit the `/etc/ssh/sshd_config` file: `sudo nano /etc/ssh/sshd_config`.
- Change the port number on line 5 from `22` to `2200`.
- Find the PermitRootLogin line and edit it to no.
- Save and exit using CTRL+X and confirm with Y.
- Restart SSH: `sudo service ssh restart`.

##### 6. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200):

```bash
$ sudo ufw default deny incoming
$ sudo ufw default allow outgoing
$ sudo ufw allow www
$ sudo ufw allow ntp
$ sudo ufw allow 2200/tcp
$ sudo ufw enable
```

##### 7. Install and configure Apache to serve a Python mod_wsgi application:

    $ sudo apt-get install apache2 libapache2-mod-wsgi-py3

##### 8. Install and configure PostgreSQL:

```
sudo apt-get install libpq-dev python3-dev
sudo apt-get install postgresql postgresql-contrib
sudo su - postgres
psql
```
##### 9. Clone the Catalog app from GitHub and Configure it:
```
cd /var/www/
sudo mkdir catalog
sudo chown grader:grader catalog
git clone https://github.com/omrkhoder/project_2.git catalog
cd catalog
nano catalog.wsgi
```
##### 10. Reload & Restart Apache Server:
```
sudo service apache2 reload
sudo service apache2 restart
```
