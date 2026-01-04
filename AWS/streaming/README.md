# Streaming

## Kinesis Data Streams
- Better for real-time processing and analytics of streaming data.
- Supports multiple consumers reading from the same stream, allowing for parallel processing and different applications to consume the same data.
- Provides fine-grained control over data retention (up to 365 days) and shard-level throughput.
- You can connect it with AWS Lambda, Kinesis Data Analytics, and other services for real-time processing, control **the batch size** in the lambda and process data in batches in real-time.

## Kinsesis Firehose
- Kinesis Firehose is more oriented towards delivering streaming data to destinations like S3, Redshift, Elasticsearch, and Splunk.
- It provides built-in data transformation capabilities using AWS Lambda, allowing you to process and transform data before delivery.
- Firehose automatically scales to match the throughput of your data stream, making it easier to handle varying data loads without manual intervention.
- It's not very suited when you need complex stream processing, real-time analytics, multiple consumers, or custom processing logic.

