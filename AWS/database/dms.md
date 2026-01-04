# AWS Database Migration Service (DMS)

## Overview
AWS DMS is a managed service that helps migrate databases to AWS quickly and securely. The source database remains fully operational during migration, minimizing downtime for applications that rely on it.

## Core Components

### Replication Instance
- Compute resource that runs the migration tasks
- Size depends on data volume and complexity
- Multi-AZ option for high availability
- Handles connection to source and target endpoints

### Endpoints
- **Source Endpoint**: Where data is migrated from
- **Target Endpoint**: Where data is migrated to
- Connection details include hostname, port, credentials, encryption settings, certificates, SSL, etc.

### Replication Tasks
- Defines what tables/schemas to migrate
- Specifies migration type
- Table mappings and transformation rules
- Can include pre-migration assessments

## Migration Types

### Full Load (One-Time Migration)
- **Description**: One-time migration of existing data from source to target
- **Use case**: Initial data migration, historical data transfer
- **Process**:
  1. Extract data from source
  2. Transfer and load into target
  3. Task completes when all data is migrated
- **Characteristics**:
  - Snapshot of data at a point in time
  - No ongoing replication after completion
  - Can be used before switching to CDC (Change Data Capture)
  - Best for non-critical systems or offline migrations

### Change Data Capture (CDC / Ongoing Replication)
- **Description**: Captures ongoing changes from source and applies to target in real-time
- **Use case**: Keep databases in sync, minimal downtime migrations
- **Process**:
  1. Reads transaction logs from source database
  2. Captures INSERT, UPDATE, DELETE operations
  3. Applies changes to target in near real-time
- **Characteristics**:
  - Continuous replication of changes
  - Low-latency data synchronization
  - Minimal impact on source database performance
  - Requires transaction logging enabled on source

### Full Load + CDC (Continuous Migration)
- **Description**: Combines both approaches for zero-downtime migration
- **Use case**: Production database migration with minimal downtime
- **Process**:
  1. **Phase 1**: Full load of existing data
  2. **Phase 2**: Simultaneously captures changes during full load
  3. **Phase 3**: Applies cached changes after full load completes
  4. **Phase 4**: Continues with ongoing CDC
- **Characteristics**:
  - Most common migration pattern
  - Database stays online during migration
  - Seamless cutover capability
  - Minimal downtime (only for final cutover)

## Change Data Capture (CDC) Deep Dive

### How CDC Works
```
Source DB Transaction Logs → DMS Replication Instance → Target DB
```

1. **Log-based CDC**: Reads database transaction logs
2. **Change Identification**: Identifies DML operations (INSERT, UPDATE, DELETE)
3. **Change Application**: Applies changes to target in order
4. **Position Tracking**: Maintains checkpoint of last processed change

### CDC Prerequisites
- Transaction logging must be enabled on source
- Sufficient privileges to read transaction logs
- Archive log retention for continuous replication
- Network connectivity between source and DMS

### Supported CDC Sources
- **Oracle**: Requires archive logging mode, LogMiner or Binary Reader
- **SQL Server**: Requires MS-CDC or MS-Replication
- **MySQL**: Requires binary logging (binlog)
- **PostgreSQL**: Requires logical replication slots
- **MongoDB**: Uses change streams
- **Amazon RDS**: Automatic backup must be enabled

### CDC Monitoring
- **CDCLatencySource**: Time between last event and current time at source
- **CDCLatencyTarget**: Time between last event applied to target
- **CDCIncomingChanges**: Number of change events waiting
- **CDCThroughput**: Changes applied per second

## Table Mappings and Transformations

### Selection Rules
```json
{
  "rules": [
    {
      "rule-type": "selection",
      "rule-id": "1",
      "rule-name": "include-schema",
      "object-locator": {
        "schema-name": "public",
        "table-name": "%"
      },
      "rule-action": "include"
    }
  ]
}
```

### Transformation Rules
- Rename schemas/tables/columns
- Add/remove columns
- Filter rows based on conditions
- Change data types
- Add prefixes/suffixes

## Best Practices

### Migration Planning
1. **Assess source database**: Size, change rate, dependencies
2. **Choose appropriate instance size**: Based on data volume and complexity
3. **Test migration**: Use test environment first
4. **Plan cutover window**: Even for CDC, plan for final validation
5. **Enable CloudWatch logging**: Monitor task progress and errors

### Performance Optimization
- Use parallel full load for large tables
- Adjust `MaxFullLoadSubTasks` parameter
- Enable Multi-AZ for production workloads
- Use LOB (Large Object) handling wisely
- Partition large tables if possible

### CDC-Specific Best Practices
- Monitor CDC latency metrics
- Ensure sufficient archive log retention
- Test failover scenarios
- Plan for network interruptions (DMS auto-resumes)
- Use validation to ensure data consistency

## Common Migration Patterns

### Homogeneous Migration
- Same database engine (e.g., Oracle to Oracle)
- Simpler, faster migration
- Minimal transformation needed

### Heterogeneous Migration
- Different database engines (e.g., Oracle to PostgreSQL)
- May require AWS SCT (Schema Conversion Tool)
- More complex transformations
- Consider data type compatibility

### Zero-Downtime Migration
1. Set up Full Load + CDC task
2. Monitor until caught up (CDC latency near zero)
3. Validate data consistency
4. Switch application connection strings
5. Monitor for issues
6. Decommission source after validation period

## Troubleshooting

### Common Issues
- **High CDC latency**: Increase instance size, check network bandwidth
- **Table not found errors**: Verify table mappings and permissions
- **LOB issues**: Adjust LOB settings (`limited-size-lob-mode`)
- **Network timeouts**: Check security groups, NAT gateway, routes
- **Validation errors**: Expected for certain data types (BLOB, CLOB)

### Monitoring and Alerts
- Task state changes
- Replication lag thresholds
- Error counts
- Storage utilization on replication instance

## Cost Considerations
- Charged by replication instance hours
- Data transfer costs (out of AWS)
- Storage for replication logs
- No charge for data transferred between AWS services in same region

## Limitations
- Some data types not supported (e.g., BFILE in Oracle)
- Large LOBs may require special handling
- DDL changes not automatically replicated during CDC
- Cross-region replication has higher latency
