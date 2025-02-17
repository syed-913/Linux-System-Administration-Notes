___
# **Cloud Computing**
### **Definition**
According to **Google**,
>"_Cloud computing is the on-demand availability of computing resources (such as storage and infrastructure), as services over the internet. It eliminates the need for individuals and businesses to self-manage physical resources themselves, and only pay for what they use._"

This definition is enough to explain **cloud computing** in most simple terms, but for clarification here's an analogy of cloud computing.

### **Assumption**

Assume a gas cylinder purchased by an individual in a house for cooking purpose, but it requires maintenance like filling at end of every week, sometimes it's nozzle gets leak or any breakage at the base which makes it imbalance etc. These problems are cost effective and are not efficient for its price.

Alternatively, that individual can purchase a gas service from a private company supplying that gas from a pipe. That gas company will take care of all the maintenance by themselves and by this the individual is free from from all the expenses and time consumptions of gas cylinder. The gas service by the gas company is more efficient without any stoppage or delay.

<center>Now what is more effective and less expensive?</center>

This Assumption above clears all the doubts about **cloud computing**. In very simple terms, Cloud computing is a collection of services offered by different vendors such as Amazon, Google, Microsoft etc.

___
Now, we can assume an actual web service based company rather than private gas company. This assumption will explain everything about cloud computing.
### **Assumption - Cloud Computing**
The assumptions in this sections will be based in scope of actual individual trying to open his own IT industry.
#### **On-Premises**

Consider a businessman named **Umar**, he wants to open an IT industry and make a good money out of it. Here's a list of physical requirements for on-premises setup of infrastructure of the company:

- Servers to host the Web/App.
- Virtual Machines.
- Private Network
- Networking Peripherals
- Databases like SAN.
- Load Balancers.
- Monitoring servers/tools.
- Database Cache.
And many more technical peripherals are required.

This on-premises infrastructure will cost up-to 40 Lakhs, also with multiple drawbacks:
- Time consumption is of 4-5 months to get all things setup.
- Cost effective.
- System Admins should have a grasp knowledge on various components and technologies required to build the infrastructure.
- And more.

#### **Co-Location**

Now, assume a company named **NetMagic**, which offers a data center to **Umar**. There would be some peripherals required by **Umar** to setup his infrastructure's space in the data center with virtual private sharing with other companies. The requirements of **NetMagic** are like servers, load balancers, database, or media storage but other equipments/services like router, internet, firewalls, power supply and more, are provide by the **NetMagic**  . Data center also has its drawbacks like non-efficient security, a shared network with other companies(VPS), less freedom and more. Although the cost expense is very much reduced from 40 lakhs to 20 lakhs also it can be setup in under 2-3 months, which is also a plus point.

<center> What is more beneficial for <b>Umar's</b> startup?</center>

___
## **Amazon Web Services**

<center>Here come's <b>Amazon!</b></center>

In 2002, **Jeff Bezos** got an idea about providing everything like servers, network, unlimited storage, and everything requires to run a website **as a service** for businesses. Although he got a lot of money from Amazon E-commerce, so he went for this investment. In the year 2004, **Jeff Bezos** starts testing on his new Idea and 2 years later he launched **Amazon Web Services** publicly and first data center for online services was built in Virginia, USA. **Jeff Bezos** introduced the new concept of "_as a service_" as [[#**Definition**|Cloud Computing]].

### **How Amazon Managed to implement Cloud Computing World Wide on a Tangible Level?**
To build Amazon Web Services on a big scale, **Jeff** requires a data center on various locations with high profile **security** to maintain data's confidentiality and global **availability** to make sure the data is available to it's customers globally. Amazon refers to this concept as **Global Infrastructure**.
#### **Global Infrastructure**
There are 3 main components in consideration of a **Global Infrastructure**:
###### **Regions**
- **Definition:** AWS regions are the geographical locations where multiple isolated Availability Zones (AZs) are present.
- **Details:**
  - There are currently 34 launched AWS regions worldwide.
  - Each region contains a minimum of 3 Availability Zones (AZs).
  - Regions are physically separated and designed for high fault tolerance and low latency.
  - Some services cannot be found in certain regions, so changing the region can be helpful.
  - While most regions have a minimum of 3 AZs, a few may have only 2. Always verify based on the AWS documentation for specific regions.
  - New regions are periodically announced, so staying updated is essential.

###### **Availability Zones (AZs)**
- **Definition:** Availability Zones are isolated locations within a region, each consisting of one or more data centers.
- **Details:**
  - Each Availability Zone has at least 2 data centers.
  - Data centers within an AZ are interconnected through high-speed, low-latency networks.
  - AZs are designed to ensure fault isolation, meaning issues in one AZ should not affect others in the same region.

###### **Edge Locations**
- **Definition:** Edge locations are geographically distributed endpoints for content delivery, acting as caching servers.
- **Details:**
  - They improve performance by reducing network latency and bringing content closer to users.
  - There are currently over 400 edge locations and 13 regional mid-tier caches, spread across 90 cities in 48 countries.
  - Edge locations are integral to services like Amazon CloudFront, which delivers content with reduced latency.
  - AWS Global Accelerator and CloudFront are the primary services benefiting from edge locations. Mentioning their role in global application performance could add context.
___
#### **Pros/Cons of Cloud Computing**

###### **Advantages**

- **Scalability:**  
    Cloud computing allows businesses to scale resources up or down based on demand. This flexibility minimizes wastage and ensures resources are available when needed.
    
- **Less Time Consumption:**  
    With cloud solutions, organizations can deploy applications and services much faster compared to traditional setups, eliminating the need to procure and set up hardware manually.
    
- **Easy Maintenance:**  
    Cloud providers handle most maintenance, including software updates, security patches, and hardware management, reducing the burden on in-house IT teams.
    
- **Security:**  
    Leading cloud providers offer advanced security features like data encryption, regular audits, and compliance with global standards, often exceeding what most organizations can implement in-house.
    
- **Performance:**  
    Cloud infrastructures are designed for high availability and speed, with features like content delivery networks (CDNs) and load balancing to ensure seamless user experiences.
    

###### **Disadvantages**

- **Expensive:**  
    While the initial costs may be lower, ongoing subscription fees for storage, computing power, and additional services can add up, especially for long-term usage.
    
- **Dependency on Internet:**  
    Cloud computing relies heavily on stable internet connectivity. Any disruptions in service can result in downtime.
    
- **Vendor Lock-In:**  
    Migrating data and applications from one cloud provider to another can be complex and costly, often locking businesses into a single provider's ecosystem.
    
- **Limited Control:**  
    Organizations using cloud solutions have less direct control over the infrastructure, as it is managed by the cloud provider.
    
- **Data Privacy Concerns:**  
    Hosting sensitive data on third-party servers may raise concerns about privacy and regulatory compliance, especially for industries like healthcare and finance.
    

---

### **Comparison Table: On-Premises vs. Co-Location vs. Cloud Computing**

|**Aspect**|**On-Premises**|**Co-Location**|**Cloud Computing**|
|---|---|---|---|
|**Definition**|IT infrastructure owned, hosted, and managed internally.|Infrastructure hosted in a third-party data center but managed by the organization.|IT resources delivered on-demand via the internet.|
|**Ownership**|Fully owned by the organization.|Organization owns the hardware; data center provides the space.|No ownership; resources are rented/subscribed.|
|**Initial Cost**|High: Requires capital expenditure for hardware and setup.|Medium: Requires hardware investment but lower setup costs.|Low: Pay-as-you-go or subscription-based.|
|**Scalability**|Limited by available hardware and resources.|Limited by the space and power provided by the data center.|Highly scalable, resources can be scaled on demand.|
|**Maintenance**|Fully managed by in-house IT teams.|Hardware managed by the organization; data center handles facilities.|Managed by the cloud provider.|
|**Performance**|High, depending on hardware specifications.|High, as the organization controls hardware but relies on data center infrastructure.|Variable; depends on the cloud provider and internet connectivity.|
|**Security**|Full control over data security measures.|Shared responsibility: Organization manages hardware security; data center manages physical security.|Advanced security provided by cloud providers but limited control.|
|**Costs Over Time**|Lower after initial investment but high for maintenance.|Medium: Hardware upgrades and co-location fees add ongoing costs.|Can become expensive with increasing resource use.|
|**Reliability**|Dependent on internal infrastructure and power.|High, as data centers have redundant power and cooling systems.|Very high with built-in redundancy and failover systems.|
|**Ideal For**|Large organizations with strict compliance and security needs.|Organizations needing control without building a data center.|Businesses needing flexibility, scalability, and lower upfront costs.|

---
<center>The choice of <b>Umar</b> depends upon his company's requirements. If he requires <b>scalability</b> than neither on-premises nor co-location rather <b>Cloud Computing</b> is the prior choice.</center>

