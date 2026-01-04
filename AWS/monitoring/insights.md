# Insights

## Container Insights

CloudWatch Container Insights is used to collect, aggregate, and summarize metrics and logs from your containerized applications and microservices. 

Container Insights is available for 
- Amazon Elastic Container Service (Amazon ECS)
- Amazon Elastic Kubernetes Service (Amazon EKS)
- RedHat OpenShift on AWS (ROSA)
- Kubernetes clusters on Amazon EC2

Container Insights supports encryption with the AWS KMS key for the logs and metrics that it collects. To enable this encryption, you must manually enable AWS KMS encryption for the log group that receives Container Insights data. This causes Container Insights to encrypt this data using the provided KMS key.
Only symmetric keys are supported. Do not use asymmetric KMS keys to encrypt your log groups.

Think of Container Insights as:
- “A pre-packaged, opinionated observability layer for ECS/EKS containers, built on top of CloudWatch.”

You get:
- Cluster-level metrics
- Service / deployment-level metrics
- Task / pod-level metrics
- Container-level metrics

Good for centralised monitoring of containerized applications.

## Lambda Insights

It gives on top
- Memory usage over time
- CPU time / load
- Network I/O
- Disk I/O / temp storage usage
- Init duration vs invocation duration (cold start vs handler time in more detail)
- Max memory used per invocation, across samples
- Detailed error metrics (out of memory, timeouts, etc)
- Lambda Duration
- Lambda Invocations
- Lambda Cold Starts

## Database Insights
Amazon RDS Performance Insights is a database performance tuning and monitoring feature that helps you quickly assess the load on your database and determine when and where to take action. It allows you to visualize database performance and analyze any bottlenecks. Performance Insights can be enabled on Amazon RDS for several database engines, including Amazon Aurora, PostgreSQL, MySQL, MariaDB, Oracle, and SQL Server. It provides a dashboard that displays database load, top SQL queries, wait events, and host-level metrics. This helps database administrators and developers identify performance issues and optimize their database workloads effectively.