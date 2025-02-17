<center>Let's take a look on <b>facebook.com</b> using Amazon services.</center>

___
### **Requirements**
To deploy an application on-premises we must require:

- **Network:** A private network for security purposes.

- **Architecture:** In that private network, a three-tier architecture to manage at least 100 users.
  - **Web Server:** A virtual machine that manages all the front-end tasks.
  - **Web Application:** A virtual machine that manages all the backend workloads.
  - **Database:** A relational database to manage all (textual data) the data input by the users like passwords, emails, phone numbers, etc.

- **Public IP:** An IP address through which users can access our website. In this case **facebook.com**.
  - There would be a public IP on which our **Web Server** is displayed and accessed by users.

- **Scaling:** If this 3-tier architecture cannot handle the load of the users, then we have to extend it further. There are 2 concepts of scaling:
  - **Horizontal Scaling:** It means adding more Web/App machines to distribute the load between them. We'll be using **Horizontal Scaling** for our Web Application.
  - **Vertical Scaling:** It means increasing the storage capacity of the Web/App servers.

- **NoSQL DB:** For a website like **facebook.com** we must require a NoSQL DB (like MongoDB, Cassandra) along with a relational database to manage website data effectively & efficiently.

- **DB Cache:** A DB cache (like Redis or Memcached) is added to manage the frequently accessed data for our relational database. It is required to avoid bottlenecking in our relational database.

- **Main File Storage:** To manage media files like in **facebook.com**'s case, pictures, videos, audios, etc. These media files (or Binary Large Objects (BLOBs)) cannot be stored in relational DBs or NoSQL DBs, so we must require unlimited media or BLOB storage (like RFS or S3). Mostly cloud storages are used for these scenarios. The metadata of these media files is stored in normal DBs.

- **Content Filter:** It is required to blur pictures and videos that have some objectionable or nudity content.

- **Click Stream Analysis (CSA):** A click stream analysis engine is used to monitor users' activities (like digital footprints left by the users on the platform) and display personalized ads. This area of **facebook.com** is for monetization purposes. I mean **facebook.com** is free, and all of its features are free. So, to monetize this free application, we must require a strategy or business model to earn money from **facebook.com**.
  - **File Storage:** A media or BLOB file storage is required for this side of the application (click stream analysis). To store the data required for displaying personalized ads to users on the platform. It is isolated from other DBs and file storages like the storages for media files of the App server and DBs for the Web server.
  - **Big Data Analysis:** For Web/Apps like **facebook.com**, it is crucial to calculate real-time insights to display personalized ads to the users or present a report of weekly most clicked/liked/viewed posts to recommend the best post to their users to hold them on to their websites. This analysis takes place on distributed systems. For example, Hadoop/Apache Spark.
  - **Data Warehouse:** A data warehouse is required to store the **CSA** logs like trending topics, which regions are more active on the platform, and more. In simple terms, all the calculations done by the analysis engine are stored in this data warehouse.
  - **Business Intelligence:** This tool is used for querying, analyzing, and generating reports for decision-making purposes in the future based on these reports and analyses. As I mentioned, this part of the organization is for monetization and improving user experience purposes.

- **Web Browser:** Another web browser (except the main one) is required so that users can access their pictures, videos, audios, etc. This Web browser gives access to the users to their personal media files.

- **2nd File Storage:** This file storage is required for **mobile users** because they cannot access the web browser directly from the browser. I mean they can, but the mobile application is easier.

- **Video Converter:** It is required for converting the exact copy of media files uploaded by the users on the web browser media file storage to mobile media file storage.

- **Content Delivery Network (Cache):** Let's assume a video goes viral on **facebook.com**, so there would be a bottleneck occurring in our main **BLOB** file storage. To avoid bottlenecks on our main media file storage, we can have a **Content Delivery Network (CDN)** cache to manage that viral video outside our network. I mean that's the frequently viewed video, so it should be managed in a cache storage device to the user's nearest **edge locations**.

- **Mobile Push Notification:** A company like **facebook.com** sends push notifications to the user's lock screen to let them know that someone sent a friend request, liked their post, and more. This concept is performed out of the network.

- **Email:** **facebook.com** also sends emails to the user's personal email address, mostly for security purposes (to ensure their integrity). These can be disabled if they're for purposes other than security.

- **Message Queue:** For messaging and chats between two or more users, **facebook.com** uses queues like first-in-first-out data structures.

- **Monitoring Dashboard:** The monitoring dashboard is used to check the health of systems, monitor malicious activities, troubleshoot errors, and manage other monitoring tasks.

<center>This is the most basic assumption of <b>facebook.com</b> requirements</center>

___
### **Services Used in this Web/App & Where**

###### **Virtual Private Cloud (VPC) - Private Network**
- To manage all the databases, servers, and other components of the Web/App under a secure environment or to maintain confidentiality within the company, we require a **Virtual Private Cloud (VPC)**. This service functions as a private network for the company, ensuring controlled and isolated access to resources.

###### **Elastic Compute Cloud (EC2) - Virtual Machines**
- Running a server on personal components is risky, so **Elastic Compute Cloud (EC2)** provides virtual machines to run Web/App servers. These **EC2** machines are cloud-based and offer an auto-scaling feature, which can automatically scale up or down based on system load, reducing downtime and optimizing costs. Virtual disks are attached to **EC2** instances:
  - **Elastic Block Store (EBS):** A virtual disk service used to store limited information related to Web/App servers.

###### **Relational Database Service (RDS) - Relational Databases**
- For managing structured relational databases, **Relational Database Service (RDS)** is used. This service supports SQL, PostgreSQL, MySQL, and other database engines to ensure efficient and scalable database management.

###### **DynamoDB - NoSQL**
- To manage unstructured data, **DynamoDB** is used as a highly scalable NoSQL database solution. It is ideal for applications requiring low-latency and high-throughput data access.

###### **ElastiCache - Database Cache**
- To handle frequently accessed data efficiently and avoid database bottlenecks, **ElastiCache** is used. It supports caching engines like Redis and Memcached, optimizing query performance.

###### **Elastic Load Balancing (ELB) - Load Balancer**
- With horizontal scaling implemented, **Elastic Load Balancing (ELB)** is used to distribute traffic among multiple machines on Web/App servers. It directs user requests to the server with the lowest CPU utilization and highest free memory.

###### **Route 53 - Domain Name Service (DNS)**
- To ensure users can access the application via a recognizable domain name, **Route 53** is used. This DNS service translates the public IP address of the server into a user-friendly domain name, such as "facebook.com."

###### **Simple Storage Service (S3) - File Storage**
- For storing media files outside relational or NoSQL databases, **Simple Storage Service (S3)** is used. This service provides unlimited storage capacity for photos, videos, and other BLOBs (Binary Large Objects).

###### **Rekognition - Content Filter**
- To filter objectionable content from media files stored in **S3**, **Rekognition** is employed. It detects objects, scenes, faces, celebrities, and inappropriate content in images and videos. It also offers facial analysis and search capabilities.

###### **Lambda - Video Converter**
- **Lambda**, a serverless computing service, is used to automate tasks such as copying videos uploaded by web browser users to the mobile application’s **S3 bucket**. This process ensures seamless access across platforms.

###### **Kinesis - Click Stream Analysis (CSA)**
- **Kinesis** captures and processes user clicks in real-time, providing actionable insights. To manage and analyze this data:
  - **S3 Bucket:** Stores the raw CSA data.
  - **Elastic MapReduce (EMR):** Used for big data analysis on the CSA data stored in **S3**.
  - **Glue - Extract, Transform, Load (ETL):** Transforms and prepares data for analysis.
  - **Redshift - Data Warehouse:** Stores processed data for long-term analysis and reporting.
  - **Quicksight - Business Intelligence:** Provides tools to create visualizations and reports for decision-making purposes.
  - **Athena:** Connects to **S3** to query unstructured data in SQL format, facilitating better BI tasks.

###### **CloudFront - Content Delivery Network**
- To cache static content like viral videos at edge locations and reduce the load on the main **S3 bucket**, **CloudFront** is used. This service enhances performance by serving content from servers closer to users.

###### **Simple Notification Service (SNS) - Mobile Push Notification**
- **SNS** enables mobile push notifications for user activities like friend requests or post likes, delivering alerts directly to user devices.

###### **Simple Email Service (SES) - Email**
- **SES** is used for sending emails to users, typically for account security purposes or activity notifications. These emails can be customized and configured to meet application needs.

###### **Simple Queue Service (SQS) - Message Queue**
- For managing real-time messaging and chat functionality, **Simple Queue Service (SQS)** provides a reliable and scalable message queue mechanism, ensuring message delivery in a first-in, first-out sequence.

###### **CloudWatch - Monitoring Dashboard**
- To monitor system health, detect malicious activities, troubleshoot errors, and ensure optimal application performance, **CloudWatch** is utilized. It provides metrics, logs, and alarms for proactive system management.

<center> We've replaced all the requirements with AWS Cloud-based Services </center>

___
### **AWS Application Services**
We've completed the basic structure of **facebook.com** with AWS, but there are many more services in use. What we've explained are the most common or basic services used in **facebook.com**. Below are some additional **Application Services** used in **facebook.com**:

###### **API Gateway - Rest API**
- **API Gateway** is used to expose backend services to third-party applications securely and efficiently. It acts as a REST API service, enabling external developers to integrate with **facebook.com**'s features while maintaining control over API requests, throttling, and security.

###### **Cognito - Web / App User Management**
- For managing user identities and authentication, **Cognito** provides web and mobile user management capabilities. It creates and manages user pools, allowing **facebook.com** to authenticate users, manage user sessions, and control access to specific resources.

###### **AppSync - Real-Time Data Synchronization**
- **AppSync** enables real-time data synchronization for applications. It powers features like live notifications and chat in **facebook.com**, ensuring seamless data updates across all connected clients.

###### **Step Functions - Workflow Orchestration**
- Complex workflows in **facebook.com** are managed using **Step Functions**. This service coordinates multiple AWS services, ensuring tasks like user verification, media processing, and content moderation are executed in sequence with failure recovery.

<center>These are the most essential <b>Application Services</b> used in a company leveraging AWS.</center>

___
### **AWS Security Services**

After discussing the most common services required to run a Web/App, we should now focus on how to **secure** it using Amazon Web Services. Maintaining confidentiality, integrity, and availability is critical. Below are the most essential security services used in Web/App architecture:

###### **IAM - Identity and Access Management**
- The primary service for managing all AWS users, access control, and identity. For instance, if a user from **EC2** wants to upload data to an **S3 bucket**, it requires permissions managed by **IAM**. This service supports role-based access control and enables granular policies for secure authentication and authorization.

###### **KMS - Key Management Service**
- To encrypt data inside databases (**RDS** or **DynamoDB**), **S3 buckets**, **EMR**, **SQS**, **Redshift**, and even **EBS**, AWS provides **KMS** or Key Management Service. **KMS** allows secure key storage and management, ensuring encryption is consistent and compliant without needing to store keys in various locations.

###### **ACM - AWS Certificate Manager**
- Users accessing the Web over HTTPS require SSL-enabled certificates. Amazon provides **ACM**, a service for provisioning, managing, and deploying SSL/TLS certificates. These certificates are primarily deployed on **ELB** or **CloudFront** to ensure secure communications.

###### **WAF - Web Application Firewall**
- To prevent attacks like XSS, SQL Injection, and DDoS, AWS offers **WAF**. This firewall protects critical services such as **API Gateway**, **ELB**, and **CloudFront**, acting as an Intrusion Prevention System (IPS) or Intrusion Detection System (IDS). **WAF** can be customized to address specific vulnerabilities through its managed rules.

###### **Inspector - Automated Vulnerability Management**
- AWS **Inspector** scans **EC2** instances for vulnerabilities, helping maintain compliance with standards such as PCI DSS or HIPAA. It detects known CVEs and provides actionable insights for patching and hardening systems. This service ensures continuous security by monitoring configurations and implementing recommended updates.

###### **Secrets Manager - Secure Credentials**
- For managing sensitive credentials like database passwords or API keys, AWS offers **Secrets Manager**. It automatically rotates secrets and integrates seamlessly with other AWS services like **RDS** and **Lambda**, ensuring sensitive information remains secure.

###### **GuardDuty - Threat Detection**
- **GuardDuty** is an intelligent threat detection service that continuously monitors AWS accounts, workloads, and data stored in **S3**. It detects malicious activities such as compromised accounts or unauthorized access attempts.

###### **CloudTrail - Audit Logging**
- To maintain accountability and ensure compliance, AWS **CloudTrail** provides detailed logging of all API calls and actions taken within an AWS account. It integrates with **CloudWatch** for real-time monitoring and alerting of suspicious activities.

###### **Shield - DDoS Protection**
- For advanced DDoS protection, AWS offers **Shield**, which comes in two tiers: Standard and Advanced. It integrates with **WAF** and other AWS services to mitigate large-scale attacks on applications and infrastructure.

<center>These are the most critical <b>Security Services</b> required in a Web/App architecture</center>

___
### **AWS Development Services**

To efficiently develop, deploy, and manage applications on AWS, Amazon provides a suite of development tools and services that streamline processes and foster collaboration among developers. Below are the key services used in a robust **AWS Development Workflow**:

###### **CloudFormation - Infrastructure as Code (IaaC)**

- **CloudFormation** is a pivotal service that allows developers to define and provision AWS infrastructure in a programmatic way. With templates written in **JSON** or **YAML**, entire environments—ranging from **EC2 instances** to **RDS databases**, **S3 buckets**, and more—can be spun up in minutes. This eliminates the need for manual setup, ensuring consistency and speeding up deployment.

###### **CodeCommit - Git Repository Service**

- Collaboration is essential in any development workflow. **CodeCommit** is AWS's fully managed Git-based version control service, enabling multiple developers to work on **Infrastructure as Code (IaaC)** templates, application code, and configuration scripts. With CodeCommit, you get secure, scalable, and versioned repositories for your code.

###### **CodeBuild - Continuous Integration (CI)**

- **CodeBuild** is an automated build service that compiles source code, runs unit tests, and generates build artifacts like executables (**.exe**) or binaries (**.bin**). It integrates with source control systems like **CodeCommit** and supports a wide range of programming languages and build tools such as **Maven**, **Gradle**, and **Ant**.

###### **CodeDeploy - Automated Deployment**

- To ensure seamless deployment of application updates, **CodeDeploy** automates the distribution of built artifacts to **EC2 instances**, **ECS clusters**, or even **on-premises servers**. It supports rolling updates and blue-green deployments, minimizing downtime and ensuring high availability during deployments.

###### **CodePipeline - Continuous Delivery (CD)**

- **CodePipeline** orchestrates the entire development workflow by integrating services like **CodeCommit**, **CodeBuild**, and **CodeDeploy** into a streamlined pipeline. It automates the steps needed to release software updates, from source code changes to production deployment, ensuring a faster and more reliable delivery process.

###### **CodeStar - Project Management and Collaboration**

- **CodeStar** provides an all-in-one interface for managing AWS-based software development projects. It integrates development tools, CI/CD pipelines, and project tracking with **AWS CloudWatch** for monitoring. CodeStar simplifies collaboration by offering features like role-based access control, tracking of code changes, and integration with **JIRA** for task management.

###### **AWS SDKs and CLI - Development Tools**

- **AWS SDKs** are available for a wide range of programming languages like Python (**Boto3**), Java, JavaScript, and Go, enabling developers to interact with AWS services programmatically.
- The **AWS Command Line Interface (CLI)** allows developers to script and automate AWS operations, making it a must-have tool for quick development and deployment tasks.

###### **Cloud9 - Integrated Development Environment (IDE)**

- **Cloud9** is a cloud-based IDE that comes pre-configured with essential development tools. Developers can write, run, and debug code directly in their browser, with built-in support for AWS CLI and SDKs, and seamless integration with other AWS services.
