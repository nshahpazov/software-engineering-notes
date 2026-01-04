---
title: AWS Notes: S3, EC2, ASG, ECS
category: Cloud & AWS
tags: [aws, s3, ec2, asg, ecs, placement-groups]
description: Mixed AWS notes on storage classes, placement groups, autoscaling, and ECS.
status: notes
---

# S3

## Object Storage Architecture
Object storage architecture is a storage architecture that manages data as objects, as opposed to file systems or block storage. Each object typically includes the data itself, metadata, and a unique identifier. This architecture is designed to handle large amounts of unstructured data, making it ideal for cloud storage solutions like Amazon S3.

S3 objects consist of
- Key
- Value
- Version ID
- Metadata

Per individual object you can store between 0 bytes and 5 TB of data. The maximum number of objects you can store in a bucket is unlimited. However, the maximum number of objects you can store in a single bucket is 100 buckets per account. You can request a limit increase for more buckets.

## S3 Storage Classes

Amazon S3 offers a range of storage classes to meet different use cases and cost requirements. The main storage classes include:
- **S3 Standard**: General-purpose storage for frequently accessed data. It offers high durability, availability, and performance.
- **S3 Intelligent-Tiering**: Automatically moves data between two access tiers (frequent and infrequent) based on changing access patterns, optimizing costs.
- **S3 Standard-IA (Infrequent Access)**: Lower-cost storage for infrequently accessed data, with a retrieval fee.
- **S3 One Zone-IA**: Similar to Standard-IA but stored in a single availability zone, offering lower costs for infrequently accessed data.
- **S3 Glacier**: Low-cost storage for archival data with retrieval times ranging from minutes to hours.
- **S3 Glacier Deep Archive**: Lowest-cost storage for long-term archival data, with retrieval times of 12 hours or more.
- **S3 Outposts**: Delivers object storage to on-premises environments using AWS Outposts, providing a consistent hybrid cloud experience.
- **S3 Archive**: A storage class designed for long-term data retention and compliance, with a focus on durability and security.


## EBS vs Instance Store
✅ EBS Provisioned IOPS: High-performance databases like PostgreSQL, Oracle, or MongoDB that require data durability.
✅ Instance Store (e.g., i3, i4, d2): Applications like Apache Spark, Elasticsearch, or Redis where speed matters more than durability.

### 
EBS Provisioned IOPS SSD is designed for I/O-intensive applications such as large relational or NoSQL databases. It is though, Block storage attached to a single instance, Not a shared file system across multiple instances and not something that “integrates with Active Directory” in any managed sense.

### Amazon FSx for Windows File Server

Amazon FSx for Windows File Server provides a fully managed native Microsoft Windows file system so you can easily move your Windows-based applications that require file storage to AWS. It is built on Windows Server and supports the SMB protocol, Windows NTFS, and Active Directory (AD) integration. It is ideal for use cases such as home directories, media workflows, and business applications that require shared file storage.


You can configure your own grace period for ASG. Default is 300 seconds. The grace period is the time that EC2 Auto Scaling waits before checking the health status of an instance after it comes into service. During this time, EC2 Auto Scaling does not count the instance as healthy or unhealthy. This allows time for the application to start and become healthy before EC2 Auto Scaling begins to check its health status.

A Placement Group is a way to control how EC2 instances are physically placed on the underlying AWS hardware to meet certain requirements — usually around performance or fault tolerance.


### EC2 Placement Group Types – Summary

| **Type**      | **Description**                                                                 | **Best For**                                                   |
|---------------|----------------------------------------------------------------------------------|----------------------------------------------------------------|
| **Cluster**   | Instances placed on the same rack for **low latency** and **high network throughput** | High-performance computing (HPC), ML, tightly coupled apps     |
| **Spread**    | Instances placed on **separate racks**, each with its own power/network          | Critical instances needing **high availability**, small fleet  |
| **Partition** | Instances grouped into **isolated partitions** (separate racks)                  | Large distributed systems (Hadoop, Kafka, Cassandra)            |

### Feature Comparison

| **Feature**               | **Cluster**     | **Spread**      | **Partition**    |
|---------------------------|-----------------|------------------|------------------|
| Optimized for             | Performance     | Availability     | Balanced (fault-tolerant large systems) |
| Rack isolation            | ❌ None          | ✅ 1 per rack     | ✅ Partition-level  |
| Multi-AZ support          | ❌ No (single AZ)| ✅ Yes           | ✅ Yes           |
| Max instances per AZ      | Hundreds+       | 7                | Hundreds+        |
| Risk of hardware failure  | High impact     | Very low impact  | Isolated per partition |
| Recommended launch timing | All at once     | Any time         | Any time         |


Some limitations you need to remember:
● You can't merge placement groups.
● An instance cannot span multiple placement groups.
● You cannot launch Dedicated Hosts in placement groups.
● A cluster placement group can't span multiple Availability Zones.



## AWS Auto Scaling

- You can use multiple Availability Zones and let EC2 Auto Scaling balance your instances across the zones.
- You can optionally associate a load balancer to the auto scaling group. You should use the health check type ELB to check the health of the instances.
- The LB can scale up and down based on the number of requests.


#### Lifecycle Hooks:
- You can use lifecycle hooks to pause instance launching or terminating and perform custom actions before the instance transitions to the next state. Example usages include:
  - Running a script to install software on the instance before it is put into service.
  - Sending a notification to an Amazon SNS topic when an instance is launched or terminated.
  - Performing custom actions such as backing up data or sending logs to a central location.
  - SSM Automation to run a script on the instance.
  - Hooks
    - autoscaling:EC2_INSTANCE_LAUNCHING
    - autoscaling:EC2_INSTANCE_TERMINATING
- Process:
    1. A lifecycle hook is triggered when an instance is launched or terminated.
    2. The instance goes into Pending:Wait or Terminating:Wait state.
    3. The instance stays in this state until the lifecycle hook is completed or the timeout period expires.
    4. You should tell the ASG to
        - CONTINUE (continue with the next step in the lifecycle)
        - ABANDON (Terminate immediately)
        - EXTEND (keep the instance in the wait state for a longer period)



### AWS ECS

- ECS is a fully managed container orchestration service that allows you to run and manage Docker containers on AWS.
- ECS supports two launch types: EC2 and Fargate.
- To connect ECS Auto Scaling with ASG you should use Capacity Providers. Capacity providers are a way to manage the infrastructure that your ECS tasks and services run on. They allow you to define how your tasks and services should scale based on the available capacity in your cluster. We connect the ECS with the ASG using the capacity provider in the middle.



## AWS ECR (Elastic Container Registry)

An Amazon ECR private registry hosts container images in a highly available and scalable architecture. There are two types of registries in Amazon ECR—private and public—and each one has different data transfer charges.

### Amazon ECR private registry
You can use an ECR private registry to manage private image repositories consisting of Docker and Open Container Initiative (OCI) images and artifacts.

- ECR is a regional service. Repos live in a single region unless you replicate them. Pulling from a different region works, but it’s slower and may cost you cross-region data transfer.

### S3 Object Lock
S3 Object Lock is a feature that allows you to store objects using a write-once-read-many (WORM) model. It can help you prevent objects from being deleted or overwritten for a fixed amount of time or indefinitely. This is useful for regulatory compliance, data retention, and protection against accidental deletion or modification.

- Retention Period: You can specify a retention period for each object, during which it cannot be deleted or modified. The retention period can be set in days or years.
- Legal Hold: Similar to the retention period, but it has no expiration date. Instead, a legal hold remains in place until you explicitly remove it. Legal holds are independent from retention periods and are placed on individual object versions.

> Object Lock works only in buckets that have S3 Versioning enabled. When you lock an object version, Amazon S3 stores the lock information in the metadata for that object version. When you lock an object version, Amazon S3 stores the lock information in the metadata for that object version. Placing a retention period or a legal hold on an object protects only the version that's specified in the request. Retention periods and legal holds don't prevent new versions of the object from being created, or delete markers to be added on top of the object. 


## S3 Retrieval Options

When you store objects in S3 Glacier or S3 Glacier Deep Archive, you have several retrieval options to choose from, each with different retrieval times and costs:

- Expedited Retrieval: This option allows you to quickly access your data when you need it urgently. Expedited retrievals typically take 1-5 minutes to complete. This option is ideal for emergency situations where you need immediate access to your data. Expedited retrievals incur higher costs compared to other retrieval options.
- Standard Retrieval: This option is suitable for most use cases where you can afford to wait a bit longer for your data. Standard retrievals typically take 3-5 hours to complete. This option offers a balance between cost and retrieval time, making it a popular choice for many users.
- Bulk Retrieval: This option is the most cost-effective way to retrieve large amounts of data from S3 Glacier or S3 Glacier Deep Archive. Bulk retrievals typically take 5-12 hours to complete. This option is ideal for non-urgent data access needs, such as large-scale data analysis or archival purposes. Bulk retrievals have the lowest cost among the three options.

## Provisioned capacity

Amazon S3 Glacier and S3 Glacier Deep Archive offer a provisioned capacity option that allows you to reserve a specific amount of retrieval capacity in advance. This is particularly useful if you have predictable retrieval needs and want to ensure that you have the necessary capacity available when you need it.

- How many expedited retrievals you can perform per day
- How much throughput you can get while doing so.

Why does it exist? Because simply using expedited retrievals on-demand can sometimes lead to throttling if many users are trying to access data at the same time. By provisioning capacity, you can avoid this issue and ensure that your expedited retrievals are processed quickly and reliably.

> Provisioned capacity is basically: “I’m paying extra so you’ll always have a lane open for my emergency restores.”
### What do you get per unit of provisioned capacity?
- at least 3 expedited retrievals every 5 minutes
- up to 150 MB/s of retrieval throughput