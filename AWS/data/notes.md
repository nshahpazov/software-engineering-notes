## Match your data stores to the business needs

Now that you have moved to serverless compute, it is important to also choose the right data stores that align with your actual business needs.

One separation of concerns you can apply is to use the CQRS pattern:

- **Command Query Responsibility Segregation (CQRS)**: This pattern separates the read and write operations into different models, allowing you to optimize each for its specific use case. For example, you could use a relational database for the write model (commands) and a NoSQL database for the read model (queries) to take advantage of their respective strengths.

| Data Stores | Characteristics and Use Cases |
|----------------|-------------------------------|
| S3 | - Flexible object storage for unstructured data, backups, and static website hosting. <br> - Data Lakes <br> - State store <br> - Filter data retreived by lambda (S3 Filter) |
| DynamoDB | - Key-value pairs <br> It can be used as an Event Store |


## S3 storage classes

- Standard S3 - frequently accessed data
### S3 Standard IA
S3 Standard-IA is for data that is accessed less frequently, but requires rapid access when needed. 
S3 Standard-IA offers the high durability, high throughput, and low latency of S3 Standard, with a low per GB storage price and per GB retrieval charge. This combination of low cost and high performance make S3 Standard-IA ideal for long-term storage, **backups**, and as a data store for disaster recovery files. 
- Infrequently accessed data that needs millisecond access
- Same low latency and high throughput as S3 Standard
- 99.9% availability

### S3 One Zone-IA
S3 One Zone-IA is for data that is accessed less frequently, but requires rapid access when needed.
Unlike other S3 Storage Classes which store data in a minimum of three Availability Zones (AZs), S3 One Zone-IA stores data in a single AZ and costs 20% less than S3 Standard-IA. Of course, for that lower price point, you give up the availability and resilience that comes from geographic redundancy. It’s a good choice for storing secondary backup copies of on-premises data or **easily re-creatable data.**
- Re-creatable infrequently accessed data
- 99.5% availability
- Same low latency and high throughput as S3 Standard

> Access pattern: infrequent but not extremely rare. Maybe monthly access.
You pay per GB stored and per GB retrieved.

### S3 Glacier Instant Retrieval
Lowest cost storage class for data that is rarely accessed and requires milliseconds retrieval. Ideal for long-term backups and disaster recovery. With S3 Glacier Instant Retrieval, you can save up to 68% on storage costs compared to using the S3 Standard-Infrequent Access (S3 Standard-IA) storage class, when your data is accessed once per quarter. 
90-day minimum storage charge per object. 
- Long-lived data that is accessed a few times per year with instant retrievals
- Millisecond retrieval
- 99.9% availability
- 128kb minimum object size
- S3 PUT API for direct uploads to S3 Glacier Instant Retrieval, and S3 Lifecycle management for automatic migration of objects
- Min storage duration 90 days

>>> Access Pattern - Very infrequent but requires instant access. Maybe quarterly access. A few times per year.
>>> Cost model - Higher retrieval cost per GB than Standard-IA and Lower storage cost per GB-month

### Amazon S3 Glacier Flexible Retrieval (Formerly S3 Glacier)

up to 10% lower cost than S3 Glacier Instant Retrieval for long-term data archiving that is accessed less frequently, like 1—2 times per year and is retrieved asynchronously. For archive data that does not require immediate access but needs the flexibility to retrieve large sets of data at no cost, such as backup or disaster recovery use cases.

- Access time: minutes to hours
- Ideal for backups and older data that is rarely accessed
- 11 9s durability
- Supports SSL for data in transit and for data at rest
- Free bulk retrievals (standard retrievals have a cost)
- You pay per GB stored (less than Instant Retrieval) and per GB retrieved ( more than Instant Retrieval)
- min storage duration 90 days

### Amazon S3 Glacier Deep Archive

Has the lowest storage cost of all S3 storage classes. Designed for customers—particularly those in highly-regulated industries, such as financial services, healthcare, and public sectors—that retain data sets for 7—10 years or longer to meet regulatory compliance requirements. 
- Data can be retrieved within 12 hours
- Ideal for long-term data retention and digital preservation
- 11 9s durability
- You pay per GB stored (lowest cost) and per GB retrieved (highest cost)
- Retrieval time 12-48 hours
- Min storage duration 180 days

