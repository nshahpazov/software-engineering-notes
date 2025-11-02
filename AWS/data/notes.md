## Match your data stores to the business needs

Now that you have moved to serverless compute, it is important to also choose the right data stores that align with your actual business needs.

One separation of concerns you can apply is to use the CQRS pattern:

- **Command Query Responsibility Segregation (CQRS)**: This pattern separates the read and write operations into different models, allowing you to optimize each for its specific use case. For example, you could use a relational database for the write model (commands) and a NoSQL database for the read model (queries) to take advantage of their respective strengths.

| Data Stores | Characteristics and Use Cases |
|----------------|-------------------------------|
| S3 | - Flexible object storage for unstructured data, backups, and static website hosting. <br> - Data Lakes <br> - State store <br> - Filter data retreived by lambda (S3 Filter) |
| DynamoDB | - Key-value pairs <br> It can be used as an Event Store |
