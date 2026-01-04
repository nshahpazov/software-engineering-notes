# Auto Scaling Modes

## Simple Scaling
- **Description**: Basic scaling based on a single metric threshold. More of a legacy option. Increase by a fixed number of instances.
- **Use case**: Simple workloads with predictable scaling needs.
- **Configuration**: Define a scaling policy with a CloudWatch alarm.

## Step Scaling
- **Description**: Scale based on multiple thresholds with different scaling adjustments. More flexible than simple scaling.
- **Use case**: Workloads with varying levels of demand.
- **Configuration**: Define multiple CloudWatch alarms and scaling adjustments.

## Target Tracking Scaling
- **Description**: Automatically adjusts capacity to maintain a target metric (e.g., CPU utilization). Use predefined or custom metrics.
- **Use case**: Most common and recommended for dynamic workloads.
- **Configuration**: Specify a target value for a metric, and Auto Scaling adjusts capacity to maintain that target.

## Predictive Scaling
- **Description**: Uses machine learning to predict future traffic and scales in advance. Requires historical data.
- **Use case**: Workloads with predictable patterns (e.g., daily or weekly cycles). Time series data is needed.
- **Configuration**: Enable predictive scaling and provide a schedule for scaling actions.

## Scheduled Scaling
- **Description**: Scale based on a predefined schedule. Useful for predictable load changes.
- **Use case**: Regular traffic patterns (e.g., business hours). Black Friday sales.
- **Configuration**: Define scaling actions with specific start and end times.

## Best Practices

- Use Target Tracking for most applications due to its simplicity and effectiveness.
- Combine Scheduled Scaling with Target Tracking for predictable workloads.
- Monitor scaling activities and adjust policies as needed.
- Test scaling policies in a staging environment before deploying to production.

## Instance Scheduler

AWS Instance Scheduler is a solution that allows you to automatically start and stop EC2 and RDS instances based on a defined schedule. This helps optimize costs by ensuring that instances are only running when needed.
- **Use case**: Non-production environments, development instances, or any workloads with predictable usage patterns.
- **Configuration**: Define schedules using tags or resource groups, and set start/stop times
- **Benefits**: Cost savings, reduced manual intervention, and improved resource management.
- Alternatives:
    - AWS Lambda functions with CloudWatch Events
    - Third-party scheduling tools
    - EventsBridge rules
    