# IAM and Access Management

## What every request looks like

Principal does **Action** on **Resource** under **Condition(Context)**
- Principal (The Who) - Who is making the request (user, role, service)
- Action - What action is being performed (e.g., `s3:GetObject`, `ec2:StartInstances`)
- Resource - The AWS resource being acted upon (e.g., specific S3 bucket, EC2 instance)
- Condition(Context) - Optional conditions that must be met for the action to be allowed (e.g., time of day, IP address)

Default is implicit deny all. Explicit allow overrides default deny. Explicit deny overrides explicit allow.

## The Five policy types
1. **Identity-based policies**: Attached to IAM users, groups, or roles (principals) to define what actions they can perform on which resources.
2. **Resource-based policies**: Attached directly to a resource (like S3 bucket or SQS queue) to define who can access them and what actions they can perform.
3. [Permissions boundaries](https://support.kion.io/hc/en-us/articles/360051051931-What-is-a-Permissions-Boundary#:~:text=A%20permissions%20boundary%20helps%20define,can%20manage%20those%20two%20services.): Advanced feature for setting the maximum permissions that an IAM entity (user or role) can have. For example, you can use permissions boundaries to ensure that a user cannot exceed certain permissions, e.g. an administrator can create users but cannot grant them full admin rights.
4. **Session policies**: Policies that are passed when assuming a role or federating a user. They further restrict the permissions of the temporary credentials issued.
5. **Service control policies (SCPs)**: Organization-level policies used in AWS Organizations to define the maximum permissions for accounts in an organization or organizational unit (OU). SCP don't grant permissions themselves; they only limit the permissions that can be granted to the accounts. You can use SCPs to define the maximum permissions for accounts in an organization or organizational unit (OU).
define the maximum permissions for accounts in an organization or organizational unit (OU).

**Mental shortcut**: Identity & Resource policies grant. Boundaries, Session, SCP filter. Trust controls who can become the caller, not what they can do to data.

```
Effective =   (Allows from identity ∪ resource)
            ∩ PermissionBoundary
            ∩ SessionPolicy
            ∩ SCP
            − AnyExplicitDeny
```


### Definitions
- **IAM User**: An entity that represents a person or service that interacts with AWS resources. It has long-term credentials.
- **IAM Role**: An entity that defines a set of permissions for making AWS service requests and can be assumed by trusted entities (users, applications, services).
- **IAM Group**: A collection of IAM users. Permissions assigned to a group are inherited by all users in that group.
- **IAM Policy**: A JSON document that defines permissions. It specifies what actions are allowed or denied on which resources.
- **Federation**: The process of allowing users from an external identity provider (like corporate directory or social identity providers) to access AWS resources without creating IAM users for each.
- **Policy**: A document that defines permissions. Policies can be attached to users, groups, or roles to specify what actions are allowed or denied.resources like S3 buckets or SQS queues to define who can access them and what actions they can perform.



### Commands

- `aws iam list-users` - Lists all IAM users in the account.
- `aws iam list-roles` - Lists all IAM roles in the account.
- `aws iam list-policies` - Lists all IAM policies in the account.