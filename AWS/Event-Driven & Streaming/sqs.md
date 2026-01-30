# Amazon SQS (Simple Queue Service)

## Overview
Amazon SQS is a fully managed message queuing service that enables you to decouple and scale microservices, distributed systems, and serverless applications.

## Queue Types

### Standard Queue
- **Throughput**: Nearly unlimited
- **Ordering**: Best-effort ordering
- **Delivery**: At-least-once delivery (messages may be delivered more than once)
- **Use case**: High throughput scenarios where occasional duplicates are acceptable

### FIFO Queue
- **Throughput**: Up to 3,000 messages per second (with batching: 300 API calls/sec × 10 messages/batch)
- **Ordering**: Strict ordering guaranteed
- **Delivery**: Exactly-once processing
- **Naming**: Must end with `.fifo` suffix
- **Use case**: When order and exactly-once processing are critical

## Key Concepts

### Message Retention
- Default: 4 days
- Range: 1 minute to 14 days
- Messages are automatically deleted after retention period

### Visibility Timeout
- Time a message is invisible after being received by a consumer
- Default: 30 seconds
- Range: 0 seconds to 12 hours
- Prevents other consumers from processing the same message
- Can be changed using `ChangeMessageVisibility` API

### Long Polling
- Reduces empty responses by waiting for messages to arrive
- Wait time: 1-20 seconds
- More cost-effective than short polling
- Enabled by setting `ReceiveMessageWaitTimeSeconds` > 0

### Dead Letter Queue (DLQ)
- Queue for messages that fail to be processed
- Used with `maxReceiveCount` parameter
- Helps debug processing issues
- Can be analyzed separately

## Message Attributes

### Standard Attributes
- **MessageId**: Unique identifier
- **ReceiptHandle**: Required for deleting/changing visibility
- **MD5OfBody**: Hash of message body
- **Timestamp**: When message was sent

### Custom Attributes
- String, Number, or Binary data types
- Maximum 10 attributes per message
- Used for message filtering and routing

## Security

### Encryption
- **In-transit**: HTTPS endpoints
- **At-rest**: KMS encryption (optional)
- SSE-SQS (SQS-managed keys) or SSE-KMS (customer-managed keys)

### Access Control
- IAM policies
- SQS access policies (resource-based)
- VPC endpoints for private access (VPC endpoints are endpoints that allow private connections between your VPC and supported AWS services without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection.)

## Integration Patterns

### Producer-Consumer
```
Producer → SQS Queue → Consumer(s)
```

### Fan-out with SNS
```
Producer → SNS Topic → Multiple SQS Queues → Different Consumers
```

### Lambda Trigger
- Lambda polls SQS queue
- Batch size: 1-10 messages (Standard), 1-10 messages (FIFO)
- Concurrent executions based on queue depth

## Best Practices

1. **Use batching**: Send/receive/delete up to 10 messages at once
2. **Implement idempotency**: Handle duplicate messages gracefully
3. **Set appropriate visibility timeout**: Based on processing time
4. **Use DLQ**: For failed message handling
5. **Monitor metrics**: Queue depth, age of oldest message, etc.
6. **Consider FIFO**: Only when strict ordering is required (lower throughput)

## Common Use Cases

- Decoupling microservices
- Buffering requests between tiers
- Load leveling and burst handling
- Asynchronous processing workflows
- Distributed task queues
- Order processing systems

## Pricing Considerations

- Charged per request (after free tier)
- Data transfer costs
- No charge for message storage within retention period
- FIFO queues same pricing as Standard queues

## Monitoring

### CloudWatch Metrics
- `ApproximateNumberOfMessagesVisible`
- `ApproximateAgeOfOldestMessage`
- `NumberOfMessagesSent`
- `NumberOfMessagesReceived`
- `NumberOfMessagesDeleted`

## Limitations

- Message size: Max 256 KB
- Message retention: Max 14 days
- Queue name: Max 80 characters
- FIFO throughput: 3,000 messages/sec with batching
- In-flight messages: 120,000 (Standard), 20,000 (FIFO)
