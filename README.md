# 🛠️ Deploying a LAMP Stack on AWS EC2 (Ubuntu 24.04 LTS) — A DevOps Implementation Guide

Welcome to this comprehensive guide and project repository showcasing the deployment of a LAMP (Linux, Apache, MySQL, PHP) stack on **AWS EC2** from scratch. This project follows DevOps best practices for configuring cloud infrastructure, securing access, hosting a web application, and automating manual steps where possible.

> 📌 **Author**: Maxwell Ugochukwu  
> 🧑‍💻 **Role**: Cloud & DevOps Engineer  
> 📆 **Date**: August 2025  
> 🌍 **Region**: Nigeria  
> 🧪 **Tested On**: Ubuntu Server 24.04 LTS (EC2)  
> 💼 **Purpose**: Showcase technical skills for DevOps/Cloud Engineer roles, cloud provisioning, and application hosting using open-source tools.

---

## 📚 Table of Contents

- [🔧 Project Overview](#-project-overview)
- [🧱 Architecture Diagram](#-architecture-diagram)
- [📦 Stack Components](#-stack-components)
- [📋 Prerequisites](#-prerequisites)
- [🚀 Deployment Steps](#-deployment-steps)
  - [Step 1: Launch EC2 Instance](#step-1-launch-ec2-instance)
  - [Step 2: Connect via SSH](#step-2-connect-via-ssh)
  - [Step 3: Install Apache2](#step-3-install-apache2)
  - [Step 4: Install MySQL](#step-4-install-mysql)
  - [Step 5: Install PHP](#step-5-install-php)
  - [Step 6: Configure Virtual Hosts](#step-6-configure-virtual-hosts)
  - [Step 7: Create Web Pages](#step-7-create-web-pages)
- [🛠️ Troubleshooting](#️-troubleshooting)
- [⚙️ Best Practices](#️-best-practices)
- [📸 Screenshots](#-screenshots)
- [🤝 Contribution](#-contribution)
- [📄 License](#-license)
- [🔗 Author & Links](#-author--links)

---

## 🔧 Project Overview

This project demonstrates a step-by-step manual deployment of a LAMP stack to:

- Provision cloud infrastructure using **AWS EC2**
- Secure access using **SSH** and firewall rules
- Serve both **HTML and PHP**-based dynamic web content
- Connect PHP to a **MySQL database**
- Expose the website to the internet using Apache2 on port 80

The deployment is ideal for beginner-to-intermediate **DevOps engineers**, **system administrators**, or **cloud enthusiasts** who want practical experience with basic hosting on AWS.

---

## 🧱 Architecture Diagram

```pgsql
┌────────────────────────────┐
│      Local Machine         │
│   (SSH + Git + Browser)    │
└────────────────────────────┘
           │
           ▼
┌────────────────────────────┐
│  AWS EC2 Instance (Ubuntu) │
│   ┌─────────────────────┐  │
│   │   Apache2 Web Server│  │
│   └─────────────────────┘  │
│   ┌─────────────────────┐  │
│   │       PHP 8.x       │  │
│   └─────────────────────┘  │
│   ┌─────────────────────┐  │
│   │   MySQL 8.x Server  │  │
│   └─────────────────────┘  │
└────────────────────────────┘
           ▲
           │
 Public IP: HTTP (Port 80)
```

---


## 📦 Stack Components

| Layer | Technology |
|-------|------------|
| **OS** | Ubuntu Server 24.04 LTS |
| **Web Server** | Apache2 |
| **Database** | MySQL |
| **Scripting Language** | PHP |
| **Cloud** | AWS EC2 |
| **SSH Tool** | Windows Terminal / OpenSSH |
| **IDE** | Visual Studio Code |
| **Version Control** | Git (GitHub) |

---

## 📋 Prerequisites

Make sure the following are installed and configured:

- ✅ Git: `https://git-scm.com/downloads`
- ✅ Visual Studio Code: `https://code.visualstudio.com/`
- ✅ Windows Terminal or any SSH-capable terminal
- ✅ AWS Account with EC2 Access(Install AWS CLI)
- ✅ OpenSSH client installed
- ✅ GitHub account to push your repository

---

## 🚀 Deployment Steps

## ✅ Step 1: Launch EC2 Instance

1. Log in to [AWS Console](https://console.aws.amazon.com/)
2. Navigate to **EC2 > Launch Instance**
![alt text](images/ec2a.png)
3. Choose AMI: `Ubuntu Server 24.04 LTS`
![alt text](images/ec2b.png)
4. Instance type: `t2.micro` (Free Tier)
5. Create a new key pair and download the `.pem` file
![alt text](images/ec2c.png)
![alt text](images/ec2d.png)
6. Configure Security group and add inbound rules:
   - SSH (port 22)
   - HTTP (port 80)
   ![alt text](images/ec2e.png)
7. Launch Your Instance: Click Launch Instance.
8. Click the instance ID to enter the ec2 
![alt text](images/ec2f.png) 
![alt text](images/ec2g.png) 
9. (Optional) Allocate and associate an Elastic IP to your instance
![alt text](images/ec2h.png)

---

## ✅ Step 2: Connect via SSH

Open terminal:

```bash
chmod 400 your-key.pem
ssh -i your-key.pem ubuntu@<your-ec2-public-ip>
```

![alt text](images/ssh1.png)

## ✅ Step 3: Install Apache2

```bash
sudo apt update
sudo apt install apache2 -y
sudo systemctl status apache2
```

![alt text](images/ssh2.png)
![alt text](images/ap1.png)
![alt text](images/ap2.png)

Test Apache:

```bash
curl http://localhost
```

![alt text](images/ap3.png)

Visit in browser:

```bash
http://<your-ec2-ip>
```

![alt text](images/ap4.png)

## ✅ Step 4: Install MySQL access the MySQL interface

```bash
sudo apt install mysql-server -y
sudo mysql
```
![alt text](images/sql.png)
![alt text](images/sql2.png)

Secure the installation and exit:

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'StrongPassword123!';
exit
```

![alt text](images/sql3.png)

Run the security script: This will prompt you with a series of questions to remove test databases, disable remote root login, and reload privilege tables to which you should reply yes for all questions.

```bash
sudo mysql_secure_installation
```

![alt text](images/sql4.png)
![alt text](images/sql5.png)
![alt text](images/sql6.png)

Log back in: You can now log into MySQL using, type your password when prompted.

```bash
sudo mysql -p
```
To exit MySQL:

```sql
exit
```

![alt text](images/sql7.png)

## ✅ Step 5: Install PHP

```bash
sudo apt install php libapache2-mod-php php-mysql -y
```
![alt text](images/php.png)

Verify PHP Installation:
This should return the PHP version information, confirming that PHP is installed.
```bash
php -v
```

![alt text](images/php2.png)

## ✅ Step 6: Configure Virtual Hosts: Next. 

Create a directory where your web project will live and ensure you have permission to manage files in that directory.

```bash
sudo mkdir /var/www/projectlamp
sudo chown -R $USER:$USER /var/www/projectlamp
```

![alt text](images/vh1.png)

Create a new virtual host config:

```bash
sudo nano /etc/apache2/sites-available/projectlamp.conf
```

![alt text](images/vh2.png)

Paste the following:

```bash
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Save and exit the  editor

![alt text](images/vh.png)

Enable the new site, test the configuration and then reload Apache:

```bash
sudo a2ensite projectlamp
sudo a2dissite 000-default
sudo apache2ctl configtest
sudo systemctl reload apache2
```

![alt text](images/vh4.png)
![alt text](images/vh5.png)

### ✅ Step 7: Navigate to the project directory and create webpages(basic HTML file):

```bash
cd /var/www/projectlamp
nano index.html
```

Paste:
```
<!DOCTYPE html>
<html>
  <head>
    <title>Hello from LAMP</title>
  </head>
  <body>
    <h1>Hello lamp from hostname</h1>
  </body>
</html>
```

![alt text](images/vh6.png)

Save and exit. You can now view this page using your EC2 public IP:

```
http://<your-ec2-public-ip>
```

![alt text](images/vh7.png)

Make PHP load before HTML: Apache usually serves index.html by default. To give priority to index.php, modify the configuration file.

```bash
sudo nano /etc/apache2/mods-enabled/dir.conf
```

![alt text](images/ep1.png)

Edit:

```bash
DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm

```

![alt text](images/ep.png)

### ➕ Add PHP Support: 
Create a PHP page:

```bash
nano /var/www/projectlamp/index.php
```

![alt text](images/ep3.png)

Paste:

```bash
<?php
phpinfo();
?>
```

![alt text](images/ep2.png)

Save and exit.

Reload Apache:

```bash
sudo systemctl reload apache2
```

Load your EC2 IP again and you should see a PHP configuration page.

![alt text](images/ep4.png)

To remove the PHP info page for security reasons after verification:

```bash
sudo rm /var/www/projectlamp/index.php
```

![alt text](images/ep5.png)

reload:

![alt text](images/ep6.png)

## 🛠️ Troubleshooting

| Issue                           | Fix                                                 |
| ------------------------------- | --------------------------------------------------- |
| `Permission denied (publickey)` | Check `chmod 400 your-key.pem`                      |
| Apache not running              | `sudo systemctl restart apache2`                    |
| `403 Forbidden` error           | Ensure proper permissions in `/var/www/projectlamp` and ensure inbound rule for port 22 and 80 are open |
| MySQL Access Denied             | Reset root password via `ALTER USER` command        |
| PHP not rendering               | Ensure `index.php` exists and Apache is restarted   |

## ⚙️ Best Practices

🔐 Use strong SSH keys and never expose .pem files

🔄 Automate deployment using Ansible or Terraform in production

🔍 Monitor server logs (/var/log/apache2/error.log)

🔧 Use .env files for PHP secrets in real apps

🔄 Enable auto-updates or patching for long-term EC2 use

#### 📄 License:
This project is licensed under the MIT License. See the LICENSE file for details.

#### 🔗 Author & Links:
Maxwell Ugochukwu
🌍 | ☁️ Cloud & DevOps Engineer
📬 [Email](princemaxwellugochukwu@gmail.com)
🔗 [LinkedIn](https://www.linkedin.com/in/maxwell-ugochukwu/)
📝 [Blog on Dev.to](https://dev.to/princemaxi/mastering-lamp-stack-implementation-in-aws-cloud-a-devops-perspective-377i)



