# Storage Options

## EBS (Elastic Block Store) - Block Storage

Amazon Elastic Block Store (Amazon EBS) provides scalable, high-performance block storage resources that can be used with Amazon Elastic Compute Cloud (Amazon EC2) instances.
The resources you can create are:
- EBS Volumes - network-attached block storage that can be attached to EC2 instances. It's not ephemeral, meaning data persists independently of the life of the instance. You use it as a hard drive for your EC2 instances.
- EBS Snapshots - point-in-time backups of EBS volumes stored in Amazon S3. 

### Features of Amazon EBS

- Multiple volume types (SSD-backed, HDD-backed) to optimize for performance and cost. SSD are for transactional workloads, HDD for throughput-intensive workloads. 
    - SSD - Good for lots of small random reads/writes. For example, boot volumes, databases, and applications that require low latency and high IOPS.
    - HDD - Good for large, sequential I/O operations. For example, big data, data warehouses, and log processing.
- Scalability
- Backup and recovery ( High availability with snapshots) - EBS Snapshots can be used to back up data, create new volumes, and replicate data across regions.
- Encryption - EBS supports encryption at rest and in transit using AWS Key Management Service (KMS).
- Performance - EBS volumes can deliver high throughput and low latency, suitable for a wide range of applications.
- Data archiving - EBS Snapshots can be stored in S3 for long-term retention and cost savings.




- Attach a disk to a server
- Block storage, low latency (in-AZ), appears as a native block device
- Zonal: the volume and instance must be in the same AZ
- Replicated within an AZ, but not “Multi-AZ storage” by itself


## FSx for ONTAP Multi-AZ + iSCSI
AWS FSx for NetApp ONTAP Multi-AZ provides a highly available and durable block storage solution using the NetApp ONTAP file system. It supports iSCSI protocol, allowing you to connect your EC2 instances to FSx for ONTAP volumes as block devices.

Think of it like managed NetApp storage in AWS that can present storage to servers as:
- Block storage via iSCSI (LUNs) - looks like a disk to Windows
- File storage via SMB/NFS, ensuring compatibility with Windows Server and related applications.
- Multi-AZ deployment
- Integrates with Microsoft Active Directory for access control
- Automatically tiers away infrequently accessed data to lower-cost storage classes
- Supports dynamic scaling of your file system capacity, can scale to accommodate petabyte-scale datasets, making it suitable for large Windows Server environments
- Delivers hundreds of thousands of IOPS and low latency for performance-intensive workloads
- Provides ONTAP data management features like snapshots, cloning, and replication
- Broadly accessible from Linux, Windows, and macOS compute instances and containers
- Supports block storage protocols like iSCSI that is commonly used in Windows environments
- simplifies migrating from on-premises NetApp systems to AWS for users currently utilizing NetApp storage


## Amazon FSx For Lustre

Amazon FSx for Lustre is a high-performance **parallel** file system optimized for fast processing of workloads such as machine learning, high-performance computing (HPC), and big data analytics. It can be linked to Amazon S3, allowing you to process data stored in S3 with low latency and high throughput.
Parallel here means that multiple compute nodes can access and read/write to the file system simultaneously, distributing the workload and improving performance for data-intensive applications.



## Storage Gateway – Volume Gateway (iSCSI) — hybrid block storage

Storage Gateway Volume Gateway provides block storage to on-premises applications using iSCSI protocol. It can be configured in two modes:
- Cached Volumes: Primary data is stored in AWS, while frequently accessed data is cached locally
- Stored Volumes: Primary data is stored locally, with asynchronous backups to AWS

It is ideal for hybrid cloud scenarios where on-premises applications need low-latency access to block storage, while leveraging AWS for durability and scalability.


## [What's the difference between block, object, and file storage?](https://aws.amazon.com/compare/the-difference-between-block-file-object-storage/)

Block storage is like a hard drive attached to your computer. It breaks data into fixed-size blocks and stores them separately, allowing for fast access and low latency. Commonly used for databases and virtual machines. Object storage is like a big filing cabinet where each piece of data is stored as an object with metadata. It is scalable and ideal for storing large amounts of unstructured data like photos, videos, and backups. File storage is like a shared network drive where data is organized in a hierarchical structure of files and folders. It is suitable for collaborative environments and applications that require file-level access, such as content management systems. 