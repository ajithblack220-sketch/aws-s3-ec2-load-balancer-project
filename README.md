# aws-s3-ec2-load-balancer-project
AWS S3, EC2 and Application Load Balancer project
AWS Cloud Infrastructure Project – S3, EC2, CloudTrail, CloudWatch & Application Load Balancer

!AWS !S3 !EC2 !Load Balancer !Monitoring

1. Project Overview

This project demonstrates the implementation of a secure and scalable cloud infrastructure using Amazon Web Services (AWS).

The primary objective is to create a private Amazon S3 bucket for file storage, monitor S3 activities using AWS CloudTrail and Amazon CloudWatch, deploy two EC2 instances running Apache web servers, and distribute incoming application traffic using an Application Load Balancer (ALB).

The project demonstrates fundamental AWS cloud computing concepts, including object storage, virtual machines, networking, security groups, load balancing, auditing, and monitoring.

2. Project Objectives

The main objectives of this project are:

Create a private Amazon S3 bucket.
Configure S3 Block Public Access.
Upload files to Amazon S3.
Monitor S3 object activities using AWS CloudTrail.
Integrate CloudTrail with Amazon CloudWatch Logs.
Create and configure an Amazon VPC and subnets.
Launch two Amazon EC2 instances.
Install and configure Apache HTTP Server.
Create security groups for EC2 and ALB.
Configure an Application Load Balancer.
Register two EC2 instances in a target group.
Configure health checks.
Verify traffic distribution between both EC2 instances.
3. AWS Services and Technologies Used
AWS Service	Purpose
Amazon S3	Private object storage
Amazon EC2	Hosting two Apache web servers
Application Load Balancer	Distributing incoming HTTP requests
AWS CloudTrail	Recording AWS API activities
Amazon CloudWatch	Monitoring and storing logs
Amazon VPC	Isolated networking environment
Public Subnets	Hosting internet-facing load balancer resources
Security Groups	Controlling inbound and outbound traffic
Target Groups	Managing backend EC2 instances
4. Project Architecture

The project follows a simple AWS architecture in which internet users access the application through an Application Load Balancer.

The ALB forwards incoming HTTP requests to a target group containing two EC2 instances.

Both EC2 instances run Apache HTTP Server and serve different welcome messages.

Amazon S3 provides private file storage, while CloudTrail and CloudWatch provide auditing and monitoring.

Architecture Diagram
text
                   INTERNET USERS
                         |
                         |
                         v
              APPLICATION LOAD BALANCER
                    HTTP : 80
                         |
                         |
                         v
                  TARGET GROUP
                 web-target-group
                    /       \
                   /         \
                  v           v
             EC2 SERVER 1  EC2 SERVER 2
               Apache        Apache
                  |             |
                  v             v
             Server 1       Server 2
             Web Page       Web Page


       S3 STORAGE AND MONITORING COMPONENTS

                 AMAZON S3 BUCKET
                 (PRIVATE ACCESS)
                         |
                         v
                  AWS CLOUDTRAIL
                         |
                         v
                 AMAZON CLOUDWATCH
                    LOG MONITORING

5. Implementation Details
Phase 1: Amazon S3 Bucket Creation

A private Amazon S3 bucket was created to store uploaded files.

Configuration:

Created an S3 bucket.
Configured the required AWS Region.
Enabled Block Public Access.
Kept the bucket private.
Uploaded sample files.
Verified successful file uploads.
Checked bucket permissions.

Security Configuration:

S3 Block Public Access was enabled to prevent unintended public access to bucket contents.

Phase 2: AWS CloudTrail Configuration

AWS CloudTrail was configured to record AWS account activity.

Implementation:

Created a CloudTrail trail.
Configured an S3 destination for trail logs.
Enabled the required S3 data event logging for object-level activity.
Selected the required S3 bucket.
Verified recorded events.

Example monitored event:

PutObject

This event records object upload activity in Amazon S3.

Phase 3: Amazon CloudWatch Configuration

Amazon CloudWatch was used to monitor CloudTrail activity.

Implementation:

Created or selected a CloudWatch log group.
Configured CloudTrail delivery to CloudWatch Logs.
Verified log streams.
Inspected recorded events.
Checked S3 object upload activities.

CloudWatch Logs helped visualize and investigate recorded AWS activities.

Phase 4: Amazon VPC and Subnet Configuration

A VPC networking environment was configured for the EC2 instances and load balancer.

Implementation:

Selected the appropriate VPC.
Configured the VPC CIDR range.
Created the required subnets.
Selected Availability Zones.
Configured network connectivity.
Associated suitable route tables.

The infrastructure was organized to support communication between the load balancer and backend EC2 instances.

Phase 5: EC2 Instance Creation

Two Amazon EC2 instances were launched.

Instance configuration:

Configuration	Value
Instance 1	web-server-1
Instance 2	web-server-2
Operating System	Amazon Linux
Web Server	Apache HTTP Server
Application Protocol	HTTP
Application Port	80

Apache was installed and enabled on both instances.

Each instance hosted a different welcome page to identify which backend server handled an incoming request.

Server 1 output:

text
AWS EC2 Load Balancer Project

Welcome to Server 1

Apache web server is running successfully.


Server 2 output:

text
AWS EC2 Load Balancer Project

Welcome to Server 2

Apache web server is running successfully.

Phase 6: Security Group Configuration

Two security groups were configured.

ALB Security Group

Purpose: Allow internet users to access the Application Load Balancer.

Type	Protocol	Port	Source
HTTP	TCP	80	0.0.0.0/0
EC2 Security Group

Purpose: Allow HTTP traffic from the Application Load Balancer to the EC2 instances.

Type	Protocol	Port	Source
HTTP	TCP	80	ALB Security Group
SSH	TCP	22	Trusted administration source or applicable EC2 Instance Connect prefix list

This configuration restricts direct HTTP access to the backend instances while allowing the load balancer to communicate with them.

Phase 7: Target Group Configuration

A target group was created to manage the backend EC2 instances.

Configuration:

Setting	Value
Target Group Name	web-target-group
Target Type	Instances
Protocol	HTTP
Port	80
Health Check Protocol	HTTP
Health Check Path	/
Success Codes	200

Both EC2 instances were registered with the target group.

Health checks initially identified a timeout issue with Server 2.

The issue was investigated by checking Apache service status, local HTTP responses, and security group rules.

After correcting the configuration, both instances successfully passed health checks.

Final result:

Instance	Health Status
web-server-1	Healthy
web-server-2	Healthy
Phase 8: Application Load Balancer Configuration

An internet-facing Application Load Balancer was created.

Configuration:

Setting	Value
Load Balancer Name	aws-project-alb
Type	Application Load Balancer
Scheme	Internet-facing
IP Address Type	IPv4
Listener Protocol	HTTP
Listener Port	80
Target Group	web-target-group

Two Availability Zones were selected with suitable public subnets.

The ALB security group was attached, and the listener was configured to forward incoming requests to the backend target group.

The load balancer became active after creation.

6. Application Testing

The application was tested using the DNS name provided by the Application Load Balancer.


The application successfully returned the welcome page from the backend EC2 instances.

Traffic Distribution Test

A total of 20 HTTP requests were sent through the ALB DNS endpoint.

The responses included:

text
Welcome to Server 1
Welcome to Server 2


This verified that both backend EC2 instances were receiving traffic through the Application Load Balancer.

Note: Application Load Balancer traffic distribution does not guarantee strict alternation between backend servers.

7. Security Best Practices

The following security practices were implemented:

Amazon S3 Block Public Access enabled.
Private S3 bucket configuration.
EC2 HTTP access restricted to the ALB security group.
ALB HTTP access configured for application users.
SSH access restricted to appropriate administration sources.
AWS CloudTrail enabled for auditing.
CloudWatch Logs used for monitoring.
AWS credentials and private keys excluded from source control.

Additional production improvements could include HTTPS using an AWS Certificate Manager certificate, IAM least-privilege policies, encryption, and enhanced monitoring.

8. Project Screenshots

Screenshots documenting the implementation are available in the screenshots/ directory.

The screenshots include:

S3 bucket creation.
S3 file upload.
S3 public access configuration.
CloudTrail trail configuration.
CloudWatch log group.
S3 PutObject event.
EC2 Server 1.
EC2 Server 2.
ALB security group.
Target group health status.
Active Application Load Balancer.
Application output through ALB DNS.
Traffic distribution test.
9. Challenges and Solutions
Challenge 1: S3 CloudWatch logs not appearing

Solution:

Verified CloudTrail trail configuration, S3 data event selectors, CloudWatch log group integration, and event delivery.

Challenge 2: VPC and subnet CIDR overlap

Solution:

Selected non-overlapping subnet CIDR ranges within the VPC CIDR range.

Challenge 3: EC2 Instance Connect SSH access

Solution:

Configured the appropriate SSH security group rule for the EC2 Instance Connect connection method.

Challenge 4: Server 2 target group health check timeout

Solution:

Checked Apache service status, verified the local HTTP response, and corrected the EC2 security group inbound rule to allow traffic from the ALB security group.

Challenge 5: Verifying load balancing

Solution:

Sent multiple HTTP requests through the ALB DNS endpoint and verified responses from both backend servers.

10. Learning Outcomes

Through this project, I gained practical knowledge of:

AWS cloud infrastructure fundamentals.
Amazon S3 bucket management.
Private object storage configuration.
CloudTrail auditing.
CloudWatch Logs monitoring.
VPC and subnet configuration.
EC2 instance deployment.
Linux server administration.
Apache web server configuration.
Security group management.
Application Load Balancer configuration.
Target group health checks.
Load balancing and traffic distribution.
Troubleshooting AWS infrastructure issues.
11. Future Enhancements

The project can be extended with:

HTTPS configuration using AWS Certificate Manager.
Auto Scaling Groups.
Route 53 DNS configuration.
AWS WAF integration.
CloudWatch alarms.
EC2 monitoring dashboards.
Infrastructure as Code using Terraform or AWS CloudFormation.
Automated deployment using CI/CD pipelines.
12. Conclusion

This project successfully demonstrates the deployment of a basic AWS cloud infrastructure using Amazon S3, Amazon EC2, AWS CloudTrail, Amazon CloudWatch, and Application Load Balancer.

A private S3 bucket was configured for file storage, AWS activity was monitored using CloudTrail and CloudWatch, and two Apache web servers were deployed on EC2.

An Application Load Balancer was configured to distribute incoming HTTP requests across both EC2 instances.

Both backend servers successfully passed health checks, and traffic distribution was verified using multiple HTTP requests.

This practical project strengthened my understanding of AWS infrastructure deployment, cloud security, monitoring, networking, and load balancing.

Project Status: Core implementation and load-balancing tests completed.

Author: Ajith Kumar V


