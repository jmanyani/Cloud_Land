# **EC2 Instance Setup \& Apache Web Server Troubleshooting**



#### **What was the goal:**



⦁Launch an AWS EC2 instance accessible via SSH and HTTP (web browser).

⦁Serve a simple web page ("Hello World") from the EC2 instance using Apache web server.



#### **Problem encountered:**



⦁Could SSH into the EC2 instance, but HTTP on port 80 was timing out.

⦁Apache web server was either not installed or not running on the instance.





#### **AWS networking misconfigurations causing blocked traffic:**

• Internet Gateway was detached from the VPC (no internet access).

• Route table missing or lacking route for 0.0.0.0/0 → IGW.

• Subnet not associated with correct route table.

• Security Group missing open port 80 (HTTP).

• Network ACL had conflicting deny rules.



#### **How did I solve it:**

✓ Attached the Internet Gateway to the VPC.

✓ Edited the Route Table to add a route sending all outbound internet traffic (0.0.0.0/0) to the Internet Gateway.

✓ Ensured the Subnet was associated with this updated Route Table.

✓ Updated the Security Group to allow inbound traffic on port 80 (HTTP) as well as port 22 (SSH).

✓ Checked and fixed the Network ACL rules to allow inbound and outbound traffic (removed conflicting deny rules).

✓ Connected to the EC2 instance via SSH using the correct .pem key and public IP.





#### **Manually installed and configured Apache:**



sudo yum update -y  

sudo yum install -y httpd  

sudo systemctl start httpd  

sudo systemctl enable httpd  


```bash
echo "<h1>Hello World from $(hostname -f)</h1>" | sudo tee /var/www/html/index.html
```



**Tested HTTP access in a browser to confirm the web page loaded successfully.**





#### **Result:**



⦁EC2 instance was accessible via SSH and HTTP.

⦁Apache web server successfully served the “Hello World” page.

⦁Networking and security configurations were correctly set up for public access.

⦁Gained clear understanding of how VPC, IGW, Route Tables, Subnets, Security Groups, and NACLs work together.









