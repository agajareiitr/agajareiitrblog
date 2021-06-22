---
title: "Aws Deploy Django"
date: 2021-06-09T10:11:55+05:30
tags: ["Python","Django","aws","ssl"]
author: "Akash Gajare"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Guide to Deploy the Django Application to AWS"
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
cover:
    image: "/aws-migration-1200x675.jpg" # image path/url
    alt: "aws-migration-1200x675.jpg" # alt text
    caption: "Source : Google Images" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page
---

## This Guide will Help you to Deploy your Django Project on AWS (amazon web services)

### Prerequisites :
1. Working Django Project Hosted on Github
2. amazon AWS Account

### Before we begin lets see how we will deploy it
1. Django Project will be Deployed on AWS EC2 Instance.
2. No S3 or Database (will use Django Default Database)
3. Buy a Domain Name (Hostinger is used).
4. use Route 53 to connect our Domain Name.
---


## This Tutorial is Divided into 6 Parts : 

## 1. Launching a EC2 instance
1. Search EC2 in Search option
2. Go to Instances and Click Launch Instance
3. Select  ```Ubuntu 20.04-amd64-server```
4. Select the ```t2.micro``` Free tier eligible
5. Go to ```Congfigure Security Group``` and click add followings rules :
    Type | Protocol | Port Range | Source
    ---- | -------- | ---------- | ------
    HTTP | TCP | 80 | 0.0.0.0/0
    HTTP | TCP | 80 | ::/0
    SSH	|TCP | 22 | 0.0.0.0/0
    HTTPS |TCP | 443 | 0.0.0.0/0

6. Click Launch Instance
7. Now After Instance Launched click on it and then Click ```Connect```
8. You will see ```EC2 Instance Connect``` and then click on ```Connect```
9. A new Tab will Open that's your Server (We can also ssh directly but in this tutorial we will use Browser only)
---
### Now that Our Instance is Launched we have do update it and make it ready for Django
---
## 2. Updating Ubuntu, Installing Python,Virtualenv, nginx, supervisor 
1. Run The Following Commands : 
    1. ```sudo apt-get update```
    2. ```sudo apt-get upgrade```

2. Check if Python3 is installed :
    1. ```python3 --version```

3. install python environment : 
    1. ```sudo apt-get install python3-venv```
    2. ```python3 -m venv Projectname-env```
4. Activate the Python Environment by ```source Projectname-env/bin/activate```
5. Install nginx ```sudo apt-get install -y nginx```
6. Install supervisor ```sudo apt-get install supervisor```
---
### Everything is Ready Now, we will clone our Repository from GitHub
---

## 3. Cloning Repository and Setting Django Project (Projectname-env should be activated):
1. run ```git clone https://github.com/agajareiitr/Todo-list-app.git``` 

        Note : here "Todo-list-app" is my repository change it with your Repository
2.  run : ```pip3 install gunicorn```
3. Install the Project requirements : ```pip3 install -r requirements.txt```
4. Go to ```cd /etc/supervisor/conf.d/``` location
5. Run Below Commands : 
    1. ```sudo touch gunicorn.conf```
    2. ```sudo nano gunicorn.conf```
    3. Paste the Below code there and Edit your `Project Name` :

            [program:gunicorn]
            directory=/home/ubuntu/Todo-list-app
            command=/home/ubuntu/env/bin/gunicorn --workers 3 --bind unix:/home/ubuntu/Todo-list-app/app.sock svportal.wsgi:application
            autostart=true
            autorestart=true
            stderr_logfile=/var/log/gunicorn/gunicorn.err.log
            stdout_logfile=/var/log/gunicorn/gunicorn.out.log
            [group:guni]
            programs:gunicorn

6. create gunicorn file :
    1. `sudo mkdir /var/log/gunicorn`
    2. `sudo supervisorctl reread`
        1. output: `guni:avaliable`
    3. `sudo supervisorctl update`
        1. output: `guni:added process group`
    4. `sudo supervisorctl status`
        1. output: `guni:gunicorn  Running pid <id>, uptime 0:00:01`

7. Go to `cd /etc/nginx/sites-available/` and run below commands : 
    1. `sudo touch django.conf`
    2. `sudo nano django.conf`
    3. Paste the Below Code (change the server name with IPv4 public address): 

            server{
            listen 80;
            server_name 65.0.130.1;
            location / {
            include proxy_params;
            proxy_pass http://unix:/home/ubuntu/Todo-list-app/app.sock;
            }
            }
    4. Now let's test the nginx service
        1. `sudo nginx -t`
        2. `sudo ln django.conf /etc/nginx/sites-enabled/`
        3. `sudo service nginx restart`

---
### Our App don't have Static Files but if your App does then follow the Below Steps
---
## 4. Connecting the Static Files :
1. Go to Path - `cd /etc/nginx/sites-enabled/`
2. Open Django.conf file by `sudo nano django.conf`
3. Paste the Below Code and Edit according to you:

        server{
        listen 80;
        server_name 65.0.130.1;
        location / {
            include proxy_params;
            proxy_pass http://unix:/home/ubuntu/Todo-list-app/app.sock;
            }
        location /static {
            autoindex on;
            alias /home/ubuntu/Todo-list-app/static;
            }
         }

4. Run nginx service again
    1. `sudo nginx -t`
    2. `sudo service nginx restart`
---
### Now Your Site is running on your IP check it, below steps are for connecting Domain
---
## 5. Connecting Domain Name with Route 53 : 

1. Go to Any Domain Name PRovider and Purchase any Domain you want
2. Go to Path - `cd /etc/nginx/sites-enabled/`
3. Open Django.conf file by `sudo nano django.conf`
4. Edit the `server_name`:
        
       server{
         listen 80;
         server_name todolist.com www.todolist.com;
         }
5. Run nginx service again
    1. `sudo nginx -t`
    2. `sudo service nginx restart`
6. Remember the ipv4 public address copy it from EC2 instance
7. in AWS search `Route53` and click Create a Hosted Zone and follow the below commands
    1. Click Create Hosted Zone and add Domain Name todolist.com (without www) and create it
    2. Click Create Record set :
            
            Record name : www
            Record type : A
            value : Your IPv4 public address
            leave else as it is and click create
    3. Click Create Record set :

            Record name : leave blank
            Record type : A
            Value : theres a Alias Button click it and choose endpoint Alias to another record in this hosted zone
            now select the www.todolist.com
            leave else as it is and click create
    4. Visit your Domain Provider and go to thier DNS servers settings
    5. In route53 theres a record name with type NS(name servers) paste them in DNS of domain provider one by one, it looks like below code :

            ns-557.awsdns-05.net
            ns-165.awsdns-20.com
            ns-1831.awsdns-36.co.uk
            ns-1154.awsdns-16.org
---
### Now your website will redirect the ipv4 to your domain name, but you can see its says now secure, so lets add a SSL Certificate
---
# 6. Adding SSL Certificate
1. Run the Following Commands : 
    1. `sudo apt-get update`
    1. `sudo apt-get install software-properties-common`
    1. `sudo add-apt-repository universe`
    1. `sudo add-apt-repository ppa:certbot/certbot`
    1. `sudo apt-get update`
    1. `sudo apt-get install certbot python3-certbot-nginx`
    1. Give the Permission required and run `sudo certbot --nginx`

2. following will be asked
    1. Email: enter your email so that you will get updates about your certificate
    2. Agree to terms of services: enter ‘a’ for accepting  
    3. Share your email: You can ignore it by entering ‘n’.
    4. Choose your domains. If you want all the domains then hit enter
    5. Choose the redirect option: Enter 2 for enabling redirect.
3. Now Restart Everything
    1. `sudo supervisorctl reload`  
    1. `sudo service nginx restart`
---
### NOTE: if your webite is not showing give some time DNS transfer requires from 10 Minutes to 48 Hours be patient.
---
## This Concludes our Tutorial Thanks