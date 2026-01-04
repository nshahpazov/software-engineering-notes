# Migration patterns

Migration patterns are strategies that organizations use to transition from one system, technology, or architecture to another. Here are some common migration patterns:

## Operational processes and development models
- Server-based - Traditional deployment on physical or virtual servers.
- Containerized - Deployment using container technologies like Docker and Kubernetes.
- APIs and Microservices - Architectural styles that structure an application as a collection of loosely coupled services.
- Serverless - Cloud computing model that allows developers to build and run applications without managing servers.

## 1. Leapfrog migration

   In this pattern, an organization skips intermediate versions of a system or technology and jumps directly to a more advanced version. This approach can save time and resources but may require significant changes to infrastructure and processes.
    * Example: Instead of containerizing an application first, an organization might move directly from a monolithic architecture to a serverless architecture, e.g. AWS Lambda. They would directly zip and upload their code to Lambda functions, bypassing the containerization step. Different endpoints would be created for each function to handle specific tasks and directly deploy them to the cloud.

## 2. Organic migration

   This pattern involves gradually transitioning to a new system or technology over time. It allows for incremental changes and reduces the risk associated with large-scale migrations. The pattern is about time and gradual adoption, not about structure.
    * Example: An organization might start by containerizing a few services of a monolithic application while keeping the rest on traditional servers. Over time, they can migrate more services to containers until the entire application is containerized.

## 3. Strangler migration

This pattern involves incrementally replacing parts of an existing system with new components until the old system is completely phased out. It allows for a smooth transition and minimizes disruption to users.
    * Example: An organization might start by creating new microservices for specific functionalities of a monolithic application. As these microservices are developed and tested, they gradually replace the corresponding parts of the monolithic application until it is fully decommissioned.

- You can start decomposing a monolithic application into microservices by identifying and extracting specific functionalities into separate services. You can start with the least critical functionalities to minimize risk or the
ones that are driving the capacity needs.
- You can use an API Gateway and an Application Load Balancer to route requests to either the monolithic application or the new microservices based on the request path or other criteria.
- You can use API Gateway multiple versions to manage different versions of your microservices and gradually phase out the monolithic application.

### How it looks in AWS terms

You might start with:

- Cron jobs → replaced by EventBridge Scheduler + Lambda.
- File uploads → moved from local disk → S3 with presigned URLs.
- Email sending → extracted into SNS + Lambda.
- Background queues → switched to SQS + Lambda consumers.