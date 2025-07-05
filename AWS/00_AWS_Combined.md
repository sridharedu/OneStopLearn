# AWS Combined Notes

## 001.AWS-what-are-regions-zones-and-edge-locations.md
✨ **AWS Regions, Zones, and Edge Locations**
- → **Regions:** AWS services are spread across the world in geographical areas called regions.
- → **Availability Zones (Zones):** Regions are divided into zones, which are locations where data centers are located.
    - → Zones are interconnected to each other.
    - → Every region will have at least two zones to ensure high availability (if one zone goes down, the other backs it up).
- → **Edge Locations:**
    - → Exist between end users/clients and the zones.
    - → They cache requests and responses to provide faster response times to end users.
    - → Instead of requests always going to the main zone, they can go to the edge location, where responses are cached and sent back quickly.

## 002.AWS-what-is-ec2.md
✨ **What is EC2**
- → EC2 stands for Elastic Compute Cloud.
- → It is an AWS service that provides the capability to launch virtual servers (called `instances` by AWS) to run applications on the cloud.

## 003.AWS-what-is-a-ami.md
✨ **What is an AMI**
- → AMI stands for Amazon Machine Image.
- → It acts as a template to launch EC2 instances.
- → An AMI includes the necessary software, such as:
    - → Operating system.
    - → Runtimes (e.g., Java runtime, Python interpreter).
    - → Databases (e.g., MySQL).
    - → Other software (e.g., Docker).
- → Any number of instances can be launched using the same AMI if similar instances are required.
- → An AMI can also be created from an existing running instance and used to launch future instances.

## 004.AWS-what-are-spot-instances.md
✨ **What are EC2 Spot Instances**
- → EC2 Spot Instances allow users to bid for unused EC2 capacity, offering significant cost savings (up to 90% compared to On-Demand instances).
- → **Key Characteristic:** AWS can reclaim a Spot Instance with a two-minute notice if the capacity is needed elsewhere or if the Spot price exceeds the user's bid.
- → **Use Case:** Suitable for fault-tolerant, flexible applications, or for quick, non-critical tasks where interruptions are acceptable.

## 005.AWS-public-vs-elastic-ip.md
✨ **Public vs Elastic IP**
- → **Public IP Address:**
    - → Automatically allocated to an EC2 instance upon launch.
    - → Changes every time the instance is stopped and started, which can be problematic for client applications.
- → **Elastic IP Address:**
    - → A static, public IP address that does not change.
    - → Can be associated with an EC2 instance.
    - → Users are charged for Elastic IPs even when they are not associated with a running instance.

## 006.AWS-what-are-ec2-instance-states.md
✨ **EC2 Instance States**
- → **Running:** The instance is actively running.
- → **Stopped:** The instance is shut down but can be started again later. You are not charged for compute time while stopped.
- → **Rebooting:** The instance is restarting.
- → **Hibernated:** The instance is temporarily put into a low-power state, preserving its in-memory contents.
- → **Terminated:** The instance is permanently deleted. All associated data on instance store volumes is lost. You are not charged for terminated instances.
- → **Difference between Stop and Terminate:**
    - → **Stop:** Allows restarting the instance later; data on attached EBS volumes persists.
    - → **Terminate:** Permanently deletes the instance; data on attached EBS volumes is also deleted unless explicitly configured otherwise.

## 007.AWS-how-to-connect-to-a-linux-instance.md
✨ **How to Connect to a Linux EC2 Instance**
- → Use SSH (Secure Shell) to connect to a remote EC2 Linux instance.
- → When launching an EC2 instance, a key pair is generated:
    - → The public key is stored on AWS.
    - → The private key (`.pem` file) is downloaded to your local machine.
- → **SSH Command:**
    - → `ssh -i /path/to/your-private-key.pem ec2-user@<instance-public-ip-or-dns>`
    - → Replace `/path/to/your-private-key.pem` with the actual path to your downloaded private key.
    - → Replace `<instance-public-ip-or-dns>` with the public IP address or DNS name of your EC2 instance.

## 008.AWS-how-to-secure-ec2-instance.md
✨ **How to Secure EC2 Instance**
- → EC2 instances are secured using `Security Groups`.
- → A Security Group acts as a virtual firewall for your instance, controlling inbound and outbound traffic.
- → **Configuring Security Group:**
    - → Navigate to the Security tab of the EC2 instance.
    - → Edit `inbound rules` to define what kind of traffic can enter the EC2 instance.
    - → **Adding a Rule:**
        - → Click "Add Rule".
        - → Select the protocol to be allowed (e.g., TCP, UDP, ICMP).
        - → Specify the port range for the protocol.
        - → Define the IP addresses or ranges that are allowed to access the instance through that protocol (e.g., `Anywhere` for public access, or custom IP ranges).

## 009.AWS-how-to-do-load-balancing.md
✨ **How to Do Load Balancing**
- → Load balancing across EC2 instances is achieved using AWS `Load Balancer` service.
- → **Steps:**
    - → Create a Load Balancer instance.
    - → Choose the type of Load Balancer: Application Load Balancer (ALB), Network Load Balancer (NLB), Gateway Load Balancer (GLB), or Classic Load Balancer (CLB).
    - → Select the EC2 instances across which the load needs to be balanced.
    - → Configure listener rules: specify on which ports the load balancer should listen (e.g., port 80) and to which ports on the instances it should redirect traffic (e.g., port 9091).
- → The Load Balancer automatically distributes incoming traffic across the registered instances.

## 010.AWS-how-to-use-auto-scaling.md
✨ **How to Use Auto Scaling**
- → Auto Scaling Groups (ASG) are used to automatically adjust the number of EC2 instances based on demand.
- → **Two-step process:**
    - → **Step 1: Create a Launch Configuration:**
        - → This defines the configuration for new instances (e.g., AMI type, instance type, security groups).
        - → It's similar to launching a single EC2 instance.
    - → **Step 2: Create an Auto Scaling Group (ASG):**
        - → Use the created launch configuration.
        - → Define a `scaling policy` within the ASG.
        - → **Scaling Policy:** Specifies rules for scaling up and down based on metrics.
            - → **Scale-up rules:** (e.g., if CPU usage is 60-70%, launch a new instance; if disk space is 80%, launch a new instance).
            - → **Scale-down rules:** (e.g., if CPU usage goes below 50%, terminate an instance).
            - → Metrics can include CPU usage, disk usage, network usage, etc.
- → This ensures instances are automatically launched when load is high and terminated when load is low.

## 011.AWS-create-custom-user.md
✨ **How to Create a Custom User (IAM)**
- → **Step 1: Go to IAM Service:** Navigate to the Identity and Access Management (IAM) service in AWS.
- → **Step 2: Create New User:**
    - → Create a new user (e.g., `dev_user`).
    - → Grant console access.
    - → Set a password (auto-generated or custom).
- → **Step 3: Assign Permissions:**
    - → Go to the permissions section.
    - → Attach existing policies or create custom policies.
    - → **Example:** Attach `EC2FullAccess` and `S3FullAccess` policies.
- → **Step 4: Share Credentials:** Share the user ID and password with the developer.
- → The developer will then be able to log in and access only the EC2 and S3 services based on the assigned policies.

## 012.AWS-what-is-sns.md
✨ **What is SNS**
- → SNS stands for Simple Notification Service.
- → It allows reacting to events happening in other AWS components and sending out notifications.
- → **Components:**
    - → **Topics:** Virtual channels that receive messages.
    - → **Subscriptions:** Endpoints subscribed to a topic that receive messages published to that topic.
- → **Notification Types:** By default, SNS supports:
    - → Email notifications.
    - → HTTP/S notifications.
    - → Triggering Lambda functions.
    - → Sending messages to SQS queues.
- → **Message Senders:** Messages can be sent by CloudWatch alarms, other AWS components, or custom microservice applications.

## 013.AWS-how-to-send-notifications.md
✨ **How to Send Notifications (e.g., for CPU Usage)**
- → To send an email notification when an instance's CPU usage or disk space is high, use AWS SNS and CloudWatch.
- → **Step 1: Create SNS Topic and Subscription:**
    - → Create an SNS topic.
    - → Create an email subscription to this topic, specifying the recipient's email address.
- → **Step 2: Create CloudWatch Alarm:**
    - → Create a CloudWatch alarm.
    - → Define the metric to monitor (e.g., CPU utilization, disk space usage).
    - → Set the threshold (e.g., if CPU usage goes above a certain percentage).
    - → Configure the alarm to publish a message to the SNS topic when the threshold is breached.
- → When the metric breaches the threshold, the CloudWatch alarm will publish a message to the SNS topic, which will then send an email notification to the subscribed email address.

## 014.AWS-what-is-cloudwatch.md
✨ **What is CloudWatch**
- → AWS CloudWatch is a monitoring and observability service.
- → It continuously collects and monitors data from various AWS services (e.g., EC2, RDS, EBS, SNS).
- → It can track metrics like CPU usage, disk space, network traffic, and more.
- → CloudWatch can analyze this data and take automated actions based on predefined alarms.
- → It integrates with almost all AWS services.

## 015.AWS-s3-vs-ebs-vs-efs.md
✨ **S3 vs EBS vs EFS**
- → **S3 (Simple Storage Service):**
    - → Object-based storage service.
    - → Data is stored in `buckets` as objects.
    - → Good for storing artifacts (JARs, WARs), application log files, backups, static website content.
    - → Not suitable for operating systems or software that requires block-level access.
- → **EBS (Elastic Block Storage):**
    - → Block-level storage service.
    - → Allows reading and writing blocks of data.
    - → Can be mounted to a single EC2 instance within a region.
    - → Suitable for operating systems, databases, and applications that require persistent, low-latency block storage.
- → **EFS (Elastic File System):**
    - → File storage service.
    - → Can be mounted to any number of EC2 instances across multiple Availability Zones within a region.
    - → Suitable for shared file systems, content management systems, and big data analytics.
    - → Good for storing operating systems and other software that needs to be accessed by multiple instances.

## 016.AWS-what-are-the-s3-storage-classes.md
✨ **S3 Storage Classes**
- → **Standard:**
    - → Offers high durability, availability, and great performance.
    - → Choose if data is accessed frequently.
    - → Data is stored across three Availability Zones.
- → **Standard-Infrequent Access (Standard-IA):**
    - → Use if data is infrequently accessed.
    - → Provides rapid access to data when required.
- → **One Zone-Infrequent Access (One Zone-IA):**
    - → Data is stored in only one Availability Zone.
    - → Reduces cost by 20% compared to Standard-IA.
    - → Data will be lost if that Availability Zone goes down.
- → **Glacier:**
    - → Provides deep archiving for data.
    - → Data can be retrieved within a few minutes or hours.
    - → A nominal fee is charged for retrieval.
- → **S3 Intelligent-Tiering:**
    - → Pick if you cannot decide which storage class to use.
    - → Uses artificial intelligence to automatically move data between access tiers based on access patterns.
    - → A small fee is charged for this intelligence.
- → **S3 Outposts:**
    - → Allows having S3 object storage directly on your premise, close to on-premise applications.
- → **Lifecycle Policy:**
    - → You can define a lifecycle policy to automatically transition objects across these storage classes as required.

## 017.AWS-what-is-cloudformation.md
✨ **What is CloudFormation**
- → CloudFormation is an Infrastructure as Code (IaC) service.
- → It allows you to create and manage the entire infrastructure required for your projects through simple YAML or JSON template files.
- → The CloudFormation template designer can be used to create these templates.

## 018.AWS-rds-vs-dynamodb.md
✨ **RDS vs DynamoDB**
- → **RDS (Relational Database Service):**
    - → Makes it easy to work with relational databases (e.g., MySQL, PostgreSQL, MSSQL, MariaDB, Amazon Aurora).
    - → Handles data replication, automatic backups, recovery, and caching features.
- → **DynamoDB:**
    - → A NoSQL database service (similar to MongoDB).
    - → Good for unstructured data, storing data as key-value pairs.
    - → AWS's version of a NoSQL database.

## 019.AWS-what-is-serverless.md
✨ **What Does it Mean to Go Serverless**
- → Serverless computing involves using cloud components to solve business problems without worrying about underlying infrastructure (servers).
- → **Key Benefits:**
    - → **No Server Management:** You don't provision, scale, or manage any servers.
    - → **Scalability & Reliability:** Non-functional requirements like scalability and reliability are handled automatically by the cloud provider.
    - → **Cost-Effective:** You pay only for what you use and how much you use it (pay-per-execution).
    - → **Speed to Market:** Faster development and deployment cycles.

## 020.AWS-what-is-aws-lambda.md
✨ **What is AWS Lambda**
- → AWS Lambda is a compute service within the AWS serverless platform.
- → It allows writing code that runs in response to events happening within other serverless components (e.g., S3, SNS, DynamoDB, SQS).
- → Similar to Java's `main` method, AWS Lambda functions have a `handler` method.
- → This `handler` method is invoked by the Lambda service when an event occurs.
- → AWS Lambda can interact with other serverless components to perform its work.
