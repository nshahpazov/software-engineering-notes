# RDS (Relational Database Service)

### When is RDS not a good fit and when to consider it?


### Failing over a Multi-AZ RDS instance

If there is a planned or unplanned outage of your writer DB instance in a Multi-AZ DB cluster, Amazon RDS automatically fails over to **a Reader database instance in a different Availability Zone (AZ).**
This ensures This ensures high availability with minimal disruption to database operations. Failover can occur during hardware failures, network issues or manual requests. **Failover times are typically under 35 seconds.** Failover completes when both reader DB instances have applied outstanding transactions from the failed writer.

#### Automatic failovers

To fail over, the writer DB instance switches automatically to a reader DB instance. to manually trigger a failover, use this command

```bash
aws rds failover-db-cluster --db-cluster-identifier mymultiazdbcluster
```

> Multi-AZ automatic failover only happens when the primary is in trouble:
- Primary storage failure
- Loss of availability on the primary