# Elastic Compute Cloud (EC2)

An EC2 instance is a virtual server in the AWS Cloud.
The instance type defines the hardware available to the instance, such as CPU, memory, storage, and networking capacity.
Amazon EC2 shares other resources of the host computer, such as the network and the disk subsystem, among instances. Each instance receiveds an equal share of the available resources. However if a resource is underutilized by other instances, your instance can use the spare capacity.

For details on instance types check this [page](./instance-types.md).

## Features of EC2

### Instances - Virtual Servers

### Amazon Machine Images (AMIs)
Preconfigured templates for your instances that package the bits you need for your server (including the OS and additional software). the AMI that you choose must be compatible with the instance type that you choose. An AMI template contains the following:
- OS + additional software
- Root volume template - the initial state of the root volume when the instance is launched. This is where the OS is booted from and installed - boot disk.
- Block device mapping, basic configs - specifies the volumes to attach to the instance when it's launched. For example an AMI might specify an EBS volume to attach as a data volume. For example, with this configuration in AMI you can specify
    - /dev/sda1 - root volume, EBS, 30 GB
    - /dev/sdb - data volume, EBS, 100 GB
    - /dev/xvdc - ephemeral storage / instance store, 50 GB

There are three ways to get an AMI:
1. Use an AMI provided by AWS or the AWS Marketplace.
2. Use an AMI shared with you by another AWS account.
3. Create your own AMI from an existing instance. This is especially useful for hardened or customized configurations where you need stronger control and security.

You can select an AMI based on the following characteristics:
- Region - the AMI must be available in the region where you want to launch the instance.
- OS (Linux, Windows, etc) - Edition (e.g., Ubuntu Server 20.04 LTS, Windows Server 2019 Base)
- Software packages - pre-installed software (e.g., LAMP stack, Docker, etc)
- Processor architecture (x86, ARM, etc) - determines the CPU instruction set used by the instance. Common architectures include x86_64 (64-bit Intel/AMD) and ARM64 (64-bit ARM).
- Launch permissions (public, shared, or private) - controls which AWS accounts can use the AMI to launch instances.
- Root volume type (Amazon EBS or instance store) - EBS-backed AMIs allow you to stop and start instances, while instance store-backed AMIs do not.
- Virtualization type (HVM or PV) - HVM (Hardware Virtual Machine) provides better performance and supports enhanced networking and GPU processing.

#### Launch Permissions
- public - The owner grants launch permissions to all AWS accounts.
- explicit - The owner grants launch permissions to specific AWS accounts, organizations, or organizational units (OUs).
- implicit - The owner has implicit launch permissions for an AMI.

#### Root volume types
- Amazon EBS-backed AMIs - The root device for an instance launched from the AMI is an Amazon EBS volume created from the AMI. You can stop and start the instance, and the data on the root volume persists. Useful for when you need to retain data or make changes to the instance over time.
- Amazon S3 backed AMI - An old relic that is not commonly used anymore. The root device for an instance launched from the AMI is an instance store volume created from a template stored in Amazon S3. You cannot stop and start the instance; you can only reboot or terminate it. Data on the root volume is lost when the instance is stopped or terminated. Amazon S3-backed AMIs are considered end of life and are not recommended for new usage.


AMIs are region specific. You can create your own AMIs, use AMIs shared with you, or use public AMIs provided by AWS or third-party vendors. To have the same AMI in multiple regions, you need to copy it to each region. Copying happens by creating a snapshot of the AMI's root volume, and then using that snapshot to create a new AMI in the target region.

### Virtualization types
- HVM (Hardware Virtual Machine) - Provides better performance by allowing the guest OS to run directly on the host hardware. Supports enhanced networking and GPU processing. Most modern instance types require HVM AMIs.
- PV (Paravirtualization) - An older virtualization type where the guest OS is aware of the hypervisor. Generally has lower performance compared to HVM and is not supported by many modern instance types.

#### AMI, EBS and snapshots
- EBS volume - the working storage volume attached to an EC2 instance. It persists independently of the instance's lifecycle.
- Snapshot - a point-in-time backup of an EBS volume. Snapshots are stored in Amazon S3 and can be used to create new EBS volumes or AMIs. Snapshots are incremental, meaning that only the blocks that have changed since the last snapshot are saved, which saves storage space and speeds up the backup process. They store the delta changes since the last snapshot.
- Snapshot -> AMI - You can create an AMI from a snapshot of an EBS volume. This is useful for creating custom AMIs with specific configurations or software installed.

You can also create an AMI from an existing instance. This is useful for creating a backup of your instance or for launching new instances with the same configuration, e.g. in ASG scaling policies.

### Instance Types
Different configurations of CPU, memory, storage, and networking capacity for your instances. Check the [instance types](./instance-types.md) page for more details.

### Amazon EBS (Elastic Block Store)
Persistent block storage volumes for use with EC2 instances. EBS volumes are network-attached and remain available after an instance is stopped or terminated. Available in various types optimized for performance and cost.

#### EBS volume types
- General Purpose SSD (gp3, gp2) - Balanced price and performance for a wide range of workloads.
- Provisioned IOPS SSD (io2, io1) - High-performance volumes for I/O-intensive applications like databases.
- Throughput Optimized HDD (st1) - Low-cost HDD for frequently accessed, throughput-intensive workloads.
- Cold HDD (sc1) - Lowest cost HDD for less frequently accessed data.
- Magnetic (standard) - Previous generation, not commonly used anymore.

### Instance store volumes
Temporary block storage volumes physically attached to the host computer. Data on instance store volumes is lost when the instance is stopped or terminated. This is often called "ephemeral storage".

### Key pairs

Secure login information for your instances using public-key cryptography. You create a key pair, and then use the private key to securely connect to your instance.

### Security groups
Virtual firewalls that control inbound and outbound traffic to your instances. You can define rules based on protocols, ports, and source/destination IP ranges. By default all inbound traffic is blocked and all outbound traffic is allowed. They are stateful, meaning if you allow an incoming request from an IP, the response is automatically allowed regardless of outbound rules. That's unlike NACLs which are stateless, meaning you have to define both inbound and outbound rules explicitly.


### DLM (Data Lifecycle Manager)

DLM is a policy engine for EBS that automates the creation, retention, and deletion of EBS volume snapshots. You can define lifecycle policies to manage snapshots based on schedules and retention rules, helping to reduce storage costs and ensure compliance with data retention requirements.


### Hibernation mode

By enabling hibernation for an EC2 instance, you can pause the instance and save its current state to the EBS root volume. When you restart the instance, it resumes from where it left off, preserving the in-memory state, running processes, and applications. This is particularly useful for instances that require a long startup time or have complex configurations that you want to avoid reinitializing. The price is reduced by not having to pay for compute while the instance is stopped, but you still pay for the EBS storage used to save the instance state and the Elastic IP Addresses attached to it. After an instance has been launched with either hiberation enabled or disabled, you cannot change this setting for that instance. You would need to create a new AMI with the desired hibernation setting and launch a new instance from that AMI and then migrate your applications and data to the new instance.

### Placement groups
Logical grouping of instances within a single AZ to optimize network performance and reduce latency. There are three types:
- Cluster placement group - packs instances close together within a single AZ for low-latency networking
- Spread placement group - spreads instances across distinct underlying hardware to reduce correlated failures
- Partition placement group - divides instances into partitions to reduce the impact of hardware failures

### Reserved Instances
A pricing option that allows you to reserve EC2 capacity for a one- or three-year term in exchange for a significant discount compared to On-Demand pricing. Reserved Instances are ideal for steady-state workloads. There are three types:
- Standard Reserved Instances - provide the highest discount but are less flexible. You can modify certain attributes like instance size within the same family, AZ, and tenancy.
- Convertible Reserved Instances - offer a lower discount than Standard RIs but allow you to change the instance type, OS, or tenancy during the term.
- Scheduled Reserved Instances - allow you to reserve capacity for specific time periods on a recurring schedule, useful for workloads that run at predictable times.

In case you need to decomission a Reserved Instance before the term ends, **you can sell it on the AWS Reserved Instance Marketplace**. This allows you to recoup some of your costs by transferring the remaining term of your Reserved Instance to another AWS customer. 

### Spot Instances
Spare EC2 capacity that AWS offers at a discounted rate compared to On-Demand pricing. Spot Instances can be interrupted by AWS with a two-minute warning when the capacity is needed for On-Demand instances. They are ideal for fault-tolerant and flexible applications, such as big data processing, containerized workloads, CI/CD, web crawling, and high-performance computing (HPC).

## Auto Scaling Groups (ASG)

### Warmup and Cooldown Periods

Auto Scaling Groups use warmup and cooldown periods to prevent rapid, unnecessary scaling actions and allow instances to stabilize. Understanding these concepts is crucial for optimizing scaling behavior and cost.

#### Default Cooldown Period

**Purpose**: Prevents ASG from launching or terminating additional instances before previous scaling activities take effect.

**How it works**:
- Applies to simple scaling policies and manual scaling
- Starts after a scaling activity completes
- During cooldown, ASG rejects scaling requests from simple scaling policies (but not from other sources like scheduled actions or target tracking)
- Default value: 300 seconds (5 minutes)

**Configuration**:
```bash
aws autoscaling update-auto-scaling-group \
  --auto-scaling-group-name my-asg \
  --default-cooldown 600
```

**Best practices**:
- Set cooldown period based on instance startup time + application initialization
- Longer cooldown for resource-intensive applications
- Shorter cooldown for fast-starting applications
- Not needed for target tracking or step scaling (they have built-in safeguards)

**Example scenario**:
1. ASG scales out by adding 2 instances at 10:00:00
2. Cooldown period starts: 10:00:00 - 10:05:00
3. During cooldown, simple scaling policies won't trigger
4. After 10:05:00, ASG can scale again based on metrics

#### Instance Warmup Period

**Purpose**: Allows newly launched instances to initialize before contributing to ASG metrics, preventing premature scale-in.

**How it works**:
- Applies to target tracking and step scaling policies
- Newly launched instances are still counted toward desired capacity
- But metrics from warming instances are excluded from CloudWatch aggregate metrics used by scaling policies
- Prevents premature scale-in because metrics appear artificially low during startup
- Default: Uses ASG's default cooldown value if not specified

**Configuration for target tracking**:
```bash
aws autoscaling put-scaling-policy \
  --auto-scaling-group-name my-asg \
  --policy-name cpu-target-tracking \
  --policy-type TargetTrackingScaling \
  --target-tracking-configuration '{
    "PredefinedMetricSpecification": {
      "PredefinedMetricType": "ASGAverageCPUUtilization"
    },
    "TargetValue": 50.0
  }' \
  --estimated-instance-warmup 300
```

**Configuration for step scaling**:
```json
{
  "MetricIntervalLowerBound": 0,
  "MetricIntervalUpperBound": 10,
  "ScalingAdjustment": 1,
  "EstimatedInstanceWarmup": 180
}
```

**Best practices**:
- Set warmup time to cover: instance boot + app initialization + health checks
- Too short: ASG may scale in prematurely
- Too long: Slower response to decreased load
- Monitor instance startup times using CloudWatch logs
- Test different values under load to find optimal duration

**Example scenario**:
```
Application startup sequence:
- Instance boot: 30 seconds
- Application startup: 60 seconds
- Load balancer health checks: 45 seconds
- Recommended warmup: 180 seconds (with buffer)
```

#### Key Differences

| Aspect | Cooldown Period | Warmup Period |
|--------|----------------|---------------|
| **Applies to** | Simple scaling, manual scaling | Target tracking, step scaling |
| **Scope** | Entire ASG | Individual instances |
| **Purpose** | Prevent rapid scaling actions | Exclude new instance metrics |
| **During period** | Blocks new scaling activities | Metrics ignored, scaling allowed |
| **Default** | 300 seconds | Uses default cooldown |
| **When configured** | ASG level | Policy level |

#### Warmup vs Cooldown Behavior

**Scenario 1: High CPU triggers scale-out**
```
With warmup:
10:00:00 - CPU > 70%, launch 3 new instances
10:00:00 - 10:03:00 - New instances warming up (metrics excluded)
10:01:00 - CPU still high, can scale again if needed
10:03:00 - Warmup ends, new instances contribute to metrics
```

**Scenario 2: Simple scaling with cooldown**
```
With cooldown:
10:00:00 - CPU > 70%, launch 2 instances
10:00:00 - 10:05:00 - Cooldown period active
10:02:00 - CPU still high, but scaling blocked by cooldown
10:05:00 - Cooldown ends, can scale again
```

#### Advanced Configuration Tips

**For microservices with fast startup**:
```
Cooldown: 60 seconds
Warmup: 90 seconds
```

**For heavyweight applications (databases, ML models)**:
```
Cooldown: 600 seconds (10 minutes)
Warmup: 900 seconds (15 minutes)
```

**For hybrid approach**:
- Use target tracking for gradual load changes (with warmup)
- Use step scaling for sudden spikes (with warmup)
- Avoid simple scaling with cooldown (legacy approach)

#### Monitoring Scaling Activities

```bash
# View scaling activities
aws autoscaling describe-scaling-activities \
  --auto-scaling-group-name my-asg \
  --max-records 20

# Check if instances are in warmup
aws autoscaling describe-auto-scaling-groups \
  --auto-scaling-group-names my-asg \
  --query 'AutoScalingGroups[0].Instances[*].[InstanceId,HealthStatus,LifecycleState]'
```

**CloudWatch metrics to monitor**:
- `GroupDesiredCapacity` - target number of instances
- `GroupInServiceInstances` - instances passing health checks
- `GroupPendingInstances` - instances in warmup
- `GroupTerminatingInstances` - instances being removed

#### Common Issues and Solutions

**Problem**: ASG scales out too aggressively
- **Solution**: Increase warmup period so new instance metrics don't artificially inflate averages

**Problem**: ASG doesn't scale when needed
- **Solution**: Reduce or remove cooldown for target tracking policies (not needed)

**Problem**: Instances terminated too quickly after launch
- **Solution**: Increase warmup period to allow full application initialization

**Problem**: Cost inefficiency from constant scaling
- **Solution**: Tune warmup/cooldown based on actual application startup time, consider scheduled scaling for predictable patterns

## Limits

### vCPU Limits
Each AWS account has default vCPU limits per region, which vary by instance family. If you need more instances, complete the Amazon EC2 limit increase request form with your use case, and your limit increase will be considered. Limit increases are tied to the region they were requested for.
