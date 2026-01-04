# AWS Databases

## Aurora and RDS

### Amazon Aurora

##### When is Aurora a good fit?
- You need many read replicas to scale read-heavy workloads.
- Your workload is spiky but requires relational database features.
- You want serverless capabilities with Aurora Serverless v2 for variable workloads.
- You need high throughput and low-latency performance for OLTP applications.

##### When is Aurora not a good fit?
- Steady workloads with predictable capacity needs (RDS may be more cost-effective).
- Simple applications that do not require advanced relational database features.


If you have an Amazon Aurora Replica in the same or a different Availability Zone, when failing over, Amazon Aurora flips the canonical name record (CNAME) for your DB Instance to point at the healthy replica, which in turn is promoted to become the new primary. Start-to-finish failover typically completes within 30 seconds. If you are running Aurora Serverless and the DB instance or AZ becomes unavailable, Aurora will automatically recreate the DB instance in a different AZ. If you do not have an Amazon Aurora Replica (i.e., single instance) and are not running Aurora Serverless, Aurora will attempt to create a new DB Instance in the same Availability Zone as the original instance. This replacement of the original instance is done on a best-effort basis and may not succeed, for example, if there is an issue that is broadly affecting the Availability Zone.

### Amazon RDS


##### When is RDS a good fit?
- You need a managed relational database with minimal operational overhead.
- You have a steady workload that requires high availability and automated backups.
- You are not pushing the limits of database scalability.
- You dont need near-instant scalability or serverless capabilities.
- Horizontal scaling with replicas for read-heavy workloads is sufficient.