🚀 AWS Auto Scaling Group Project (111025)

This project demonstrates how I deployed a scalable and fault-tolerant web application on AWS using the AWS Management Console (no Terraform or CLI).
It replicates a real-world infrastructure that ensures uptime, automatic recovery, and load balancing for web applications.

🧩 Overview

The goal was to build an auto-scaling environment that launches EC2 instances behind an Application Load Balancer (ALB) using a Launch Template and User Data script.

Each instance automatically hosts a simple web page that displays instance metadata and a GIF — confirming that auto-scaling, networking, and user data all work correctly.

Through this project, I learned how to:

Deploy EC2 instances using a Launch Template

Automatically scale with an Auto Scaling Group (ASG)

Configure Security Groups for layered access control

Use Target Groups for intelligent traffic routing

Verify successful deployment via a working public DNS

Visualize architecture inside the AWS VPC console

🏗️ Architecture Summary
Component	Purpose
VPC (Virtual Private Cloud)	Isolates all cloud resources inside a secure network
Subnets	Split resources across Availability Zones (for redundancy)
Security Groups	Define inbound/outbound traffic rules
EC2 Instances	Host the simple web page and run Apache
Launch Template	Defines AMI, instance type, and user data configuration
Auto Scaling Group	Maintains availability and scales automatically
Target Group	Sends traffic to healthy EC2 instances
Application Load Balancer	Routes web traffic to the ASG
Public DNS	Confirms the setup is functional via browser access
⚙️ Step-by-Step Implementation

1️⃣ VPC and Subnets
<img width="1872" height="881" alt="111025vpc screenshot" src="https://github.com/user-attachments/assets/fada1a69-5aea-402b-92aa-6471537c4a68" />


Created a custom VPC with two public and two private subnets across different AZs (us-east-1a and us-east-1b) for high availability.
📸 VPC Layout Screenshot


2️⃣ Security Groups


Set up two Security Groups:

Load Balancer SG: allows inbound HTTP (80)

EC2 SG: allows inbound traffic only from the Load Balancer SG

This design prevents direct access to EC2 instances, forcing all traffic through the load balancer.


3️⃣ Launch Template


Created a Launch Template named 111025launchtemplate that included:

AMI: Ubuntu 20.04 LTS

Instance Type: t3.micro

User Data Script: Installs Apache and deploys a custom HTML file showing instance metadata and a Steph Curry GIF.

📸 Launch Template Screenshot
<img width="1902" height="912" alt="111025launchtemplate screenshot" src="https://github.com/user-attachments/assets/d9d39cb7-3329-4b6c-b768-5cd5154dec84" />



4️⃣ Auto Scaling Group

Created an ASG named 111025autoscalinggroup with:

Min: 1

Desired: 1

Max: 6

This ensures that if an instance is terminated, a new one automatically replaces it.
📸 ASG Screenshot
<img width="1906" height="927" alt="111025autoscalinggroup" src="https://github.com/user-attachments/assets/fd41d959-6390-45d9-99cf-582dfa433248" />



5️⃣ Target Group

Configured a Target Group (111025targetgroup) to register healthy instances and route web traffic.
📸 Target Group Screenshot
<img width="1897" height="922" alt="111025targetgroup" src="https://github.com/user-attachments/assets/00cbb4d6-d73c-4d0e-93bb-b8ebc30c733e" />


6️⃣ Load Balancer

Created an Application Load Balancer named 111025loadbalancer to distribute traffic between EC2 instances in multiple AZs.
📸 Load Balancer Screenshot
<img width="1917" height="872" alt="111025loadbalancer" src="https://github.com/user-attachments/assets/b062fd5c-b9a5-4f4e-898e-3898b4567ff9" />


7️⃣ EC2 Instances

Verified that EC2 instances launched correctly from the Launch Template and registered with the Target Group.
📸 EC2 Screenshot
<img width="1856" height="881" alt="111025ec2 screenshot" src="https://github.com/user-attachments/assets/cde307cb-264a-44f2-a976-507c482eada9" />


8️⃣ Web Verification

Accessed the load balancer DNS name to verify successful deployment:

📸 Public DNS Screenshot
<img width="1472" height="961" alt="111025dns screenshot" src="https://github.com/user-attachments/assets/e0736430-6069-4080-bfd6-7891e51572ee" />


Public URL Example:

http://111025loadbalancer-1637508158.us-east-1.elb.amazonaws.com

🧠 Challenges & Solutions
Challenge	Solution
Ensuring EC2 launched in multiple AZs	Selected subnets from us-east-1a and us-east-1b during ASG creation
Testing connectivity safely	Restricted direct SSH access and used layered Security Groups
Verifying ASG behavior	Terminated one instance to confirm automatic replacement
Browser access issues	Adjusted Security Group inbound rules to allow HTTP (port 80)
✅ Key Takeaways

Learned how Launch Templates, Auto Scaling Groups, and Load Balancers work together.

Gained real experience setting up high availability in AWS.

Reinforced the importance of network segmentation and security best practices.

Successfully deployed a self-healing web environment with visible proof (live DNS page).

🧭 Next Steps

Add HTTPS (SSL) via an ACM certificate

Implement CloudWatch alarms for auto scaling based on CPU usage

Replace static HTML with a WordPress deployment hosted behind the ASG

💵 Teardown & Cost Management

After project completion, I deleted all resources to avoid charges:

✅ Removed:

Auto Scaling Group

Launch Template

EC2 Instances

Target Group

Load Balancer

Security Groups

VPC and Subnets

💡 Lesson: Good cloud engineers always clean up test environments to stay cost-efficient and compliant.

🧰 Tools Used

AWS Management Console

EC2, ALB, ASG, VPC, CloudWatch, IAM

Ubuntu Server 20.04 LTS

Apache2 (via User Data script)
