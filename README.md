# NUPN Stack Architecture

**Credits: Roberto Russo, Cloud Architect**

![NUPN architecture diagram](https://i.ibb.co/c2tjzgC/NUPN-architecture-diag-drawio.png)

The NUPN Stack Architecture is a **robust and scalable architecture**, ideal for modern web applications. **It comprises NodeJS, Ubuntu, PM2, and Nginx (NUPN)**, which collectively create a powerful and efficient environment for deploying and running applications. This architecture is deployed on an Amazon Machine Image (AMI), based on the latest Long Term Support (LTS) version of Ubuntu.

## Stack Components

### NodeJS
NodeJS provides a high-performance, event-driven non-blocking I/O model, ideal for real-time applications. 

### Ubuntu
Ubuntu's LTS (24.04) ensures system stability and security, providing a reliable base for our applications. 

### PM2
PM2 is a production process manager for Node.js applications with a built-in load balancer, ensuring our applications are always alive, auto-restarting and facilitating common admin tasks. 

### NginX
Nginx is a high-performance HTTP server and reverse proxy, which also provides benefits of load balancing and HTTP caching.

## Scalability
While the given example involves a single application, **it can easily be scaled out or in**, integrating seamlessly with other AWS services such as the Application Load Balancer (ALB), Auto Scaling Groups (ASG), or Route 53 (R53), thereby providing a comprehensive and robust solution for modern-day applications.

In this example, we'll be deploying a simple project providing a simple but effective architecture configuration made on Terraform. On port 80 (HTTP), we'll use NginX to reverse proxy localhost:3000, which is the local port for NodeJS projects, to our server.

## 📕 Explanation

In the Terraform configuration, we create the following resources:

Before building the Terraform configuration, ensure that AWS Access Keys are configured properly in your system. Pay attention to the AWS region selected during the `aws configure`.

1. Create/import your .pem Key Pair from the AWS Console, and then copy it to your Terraform project folder.
2. The Terraform configuration includes:
    - A Virtual Private Cloud (VPC) with a single public subnet and one private subnet.
    - An Internet Gateway.
    - A route table, which assigns the Internet Gateway to the public subnet, allowing it to connect to the internet.
    - A security group that opens incoming TCP ports 22, 80, 443, and 3000 to 0.0.0.0/0 and allows all outgoing traffic.

    *Note: Traffic is opened towards 0.0.0.0/0 only for example purposes. Once created, these settings can be modified according to your requirements from the AWS Console.*

    - An EC2 instance is then created from my custom NUPN Stack AMI 🏗️
    - The previously created security group is associated with this instance.
    - A key pair created from the AWS console is also associated with it.

### Instructions

1. Clone the repo 
2. Copy the Key Pair (.pem) to the same folder.

For running the configuration, open the terminal and use the following commands:

1. `terraform init` to initialize your Terraform configuration.
2. `terraform plan` to ensure the resources are correct.
3. `terraform apply` and input 'yes' when needed to apply the changes.

If you want to delete everything, you can use `terraform destroy` and input 'yes'.

Now you’re ready to rock with your NUPN stack 🚀

## Example Project Deployment

For example purposes I’ll deploy the easiest NodeJS+Express project in the face of Earth 😅

- Connect to your Instance (from EC2 instance Connect or your terminal) and start a node project

```bash
mkdir example-app
cd example-app
npm init # Go through the NPM configuration
npm install express --save
touch index.js
nano index.js ##copy this code below

var express = require('express');
var app = express();
app.get('/', function (req, res) {
  res.send('Hello World- NUPN Stack Example');
});
app.listen(3000, function () {
  console.log('Example app: NUPN-Stack');
});
# run the app
node index.js
# it will be visible on http://ec2_public_ip:3000/ but now let's use pm2 and nginx
pm2 start index.js --name express -- start
sudo systemctl restart nginx
# now you can navigate to your http://ec2_pub_ip/ and see the nodeJS Prod app!
```

## 🔐Future Plans and Access to the AMI
The AMI for this setup is available upon request. Please contact me if you’re really interested.

In the future, the AMI will be made public for the AWS community ☁️

### Contact Information

- **Contact**: roberto.russo@thewavestudio.it
- **Role**: Cloud Architect

By following these steps, you will have a robust and scalable environment for deploying your Node.js applications using the NUPN stack on AWS.
