___
## **AWS Global Infrastructure**
I've discussed **Global Infrastructure** in detail in the linked note below:

[[Cloud Computing - AWS#**Global Infrastructure**|Link to in-depth look at Global Infrastructure]]
### **Services**
- **Overview:** AWS offers a wide range of over 200 fully-featured cloud-based services.
- **Categories and Examples:**
  - **Compute:** EC2 (Elastic Compute Cloud), Lambda, Elastic Beanstalk.
  - **Storage:** S3 (Simple Storage Service), EBS (Elastic Block Store), Glacier.
  - **Databases:** RDS (Relational Database Service), DynamoDB, Redshift.
  - **Networking:** VPC (Virtual Private Cloud), Route 53, CloudFront.
  - **Developer Tools:** CodeCommit, CodeDeploy, CodePipeline.
  - **IoT:** AWS IoT Core, Greengrass.
  - **Security:** IAM (Identity and Access Management), KMS (Key Management Service), CloudTrail.
  - **Machine Learning:** SageMaker, Rekognition, Polly.
  - AWS continuously adds new services and features. The exact number of services may vary over time.
  - Examples of interaction between services: S3 working with Lambda for serverless applications.

___
## **AWS Account, Users and Services Scope**

### **Accounts**
When a new account is created on AWS, by default amazon put the user in a specific region. Although the account user has all the access over all the services purchased by his account. Account on AWS is not limited to a certain region but it can run any service all over the world.

Assume an **EC2** machine created in the default region (for example Virginia), that exact same **EC2** machine will not be created in other regions like Mumbai. We have to create a new machine in the Mumbai region.
___
### **Users**
Amazon offers two kinds of users just like in Linux:
##### **Root User**
- Root is the user who's in-charge of everything and can deploy any service at any time. It has the most privilege among other users.
##### **IAM User**
- Identity and Access management user is the normal user. It has limited access but its access and identity can be managed by root user.
___
### **Services Offered by Amazon**

##### 1. **Compute**

- **Amazon EC2** - Scalable virtual servers in the cloud.
- **AWS Lambda** - Serverless compute for running code in response to events.
- **Amazon ECS (Elastic Container Service)** - Managed container orchestration for Docker containers.
- **Amazon EKS (Elastic Kubernetes Service)** - Managed Kubernetes service for containerized applications.
- **AWS Batch** - Managed service for batch computing jobs.

##### 2. **Storage**

- **Amazon S3** - Object storage for unlimited data storage and retrieval.
- **Amazon EBS (Elastic Block Store)** - Block storage for EC2 instances.
- **Amazon FSx** - Managed file systems for Windows, Lustre, and other workloads.
- **Amazon Glacier** - Low-cost archival storage with retrieval flexibility.
- **AWS Storage Gateway** - Hybrid storage service for on-premises and cloud integration.

##### 3. **Databases**

- **Amazon RDS** - Managed relational databases for MySQL, PostgreSQL, and others.
- **Amazon DynamoDB** - Fully managed NoSQL database with high scalability.
- **Amazon Aurora** - High-performance relational database compatible with MySQL and PostgreSQL.
- **Amazon ElastiCache** - In-memory caching service for Redis and Memcached.
- **Amazon Redshift** - Cloud data warehousing solution for large-scale analytics.

##### 4. **Networking**

- **Amazon VPC** - Isolated cloud networking for resource security.
- **AWS Direct Connect** - Dedicated network connection between on-premises and AWS.
- **Amazon Route 53** - Scalable DNS and domain registration service.
- **AWS Elastic Load Balancing (ELB)** - Automatically distributes network traffic across resources.
- **AWS CloudFront** - Content delivery network (CDN) for low-latency content distribution.

##### 5. **Monitoring and Management**

- **Amazon CloudWatch** - Monitor resources, logs, and metrics in real-time.
- **AWS CloudTrail** - Tracks user activity and API calls across AWS accounts.
- **AWS Config** - Tracks configuration changes in resources for compliance.
- **AWS Systems Manager** - Unified management and automation for AWS resources.
- **AWS Trusted Advisor** - Provides recommendations for cost, performance, and security optimization.
___