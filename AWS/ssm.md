# AWS Systems Manager (SSM)

AWS Systems Manager (SSM) is AWS’s operations “control plane” for managing fleets of compute across AWS and hybrid environments. It helps you **automate**, **patch**, **configure**, **access**, and **audit** instances at scale—often **without opening inbound ports** or maintaining bastion hosts.

SSM is a frequent SAA topic because it’s the “AWS-native” answer to:
- **Patch management** and OS maintenance at scale
- **Running commands** across many instances safely
- **Secure access** to instances without SSH/RDP exposure
- **Storing configuration** (parameters) centrally
- **Keeping instances in a desired state** (drift control)
- **Operational automation** via runbooks

---

## Core Concepts (SAA-friendly)

### Managed Instances
A **managed instance** is any server that Systems Manager can manage:
- EC2 instances
- On-premises servers / VMs (hybrid)
- Instances in other clouds (as long as the agent + connectivity are in place)

### SSM Agent
Most EC2 AMIs already include the **SSM Agent**. If not, you install it.
- The agent is what receives instructions (Run Command, Session Manager, Inventory collection, etc.)

### IAM: Instance Profile Is Required
Your EC2 instances typically need an **instance profile (IAM role)** with permissions like:
- `AmazonSSMManagedInstanceCore` (common baseline managed policy)

Without the right IAM role, the agent can’t register and you won’t be able to manage the instance.

### Network Connectivity: Outbound Only (Usually)
SSM is designed so you **don’t need inbound access** to the instance.
- Instances need **outbound HTTPS (443)** access to SSM endpoints.
- In private subnets with no internet access, you can use **VPC Interface Endpoints (PrivateLink)** for:
  - `ssm`
  - `ec2messages`
  - `ssmmessages` (Session Manager)
  - plus (often) `logs` and `kms` depending on your setup

### SSM Documents
SSM actions are defined by **Documents**:
- **Command documents** (`Command`) — used by Run Command
- **Automation documents** (`Automation` runbooks) — orchestration workflows
- **Policy documents** (less common for SAA focus; used for applying certain managed policies/configurations)

---

## Key Features

## Automation (Runbooks)
**Automation** orchestrates multi-step operational workflows:
- Patch fleets, restart services, create AMIs, rotate instances, remediate incidents
- Can call AWS APIs (e.g., stop/start instances, modify security groups, create snapshots)
- Supports approvals, parameters, branching, and error handling

**SAA exam angle**
- Use Automation when you need a **workflow**, not just a one-liner command.
- Great for “least management overhead” operational tasks (prebuilt runbooks exist).

---

## Run Command
**Run Command** executes shell commands or scripts on managed instances:
- No SSH required
- Runs across one instance or thousands
- Supports targeting by instance IDs, tags, resource groups

**SAA exam angle**
- Best answer for: “run commands across fleet quickly without logging into instances.”

---

## Session Manager
**Session Manager** provides interactive shell access (and port forwarding) to instances **without opening inbound ports** or using SSH keys/bastions.

Key benefits:
- No inbound 22/3389 needed
- Centralized access control via IAM
- Session logging possible (CloudWatch Logs / S3)
- Works well in private subnets (with VPC endpoints)

**SAA exam angle**
- If the prompt says “secure access without bastion host / without opening inbound ports / avoid SSH keys,” Session Manager is usually the answer.

---

## Patch Manager
**Patch Manager** automates patching for OS and applications (depending on platform support).

Core building blocks:
- **Patch baselines**: define what patches are approved/blocked (AWS provides defaults; you can customize)
- **Patch groups**: group instances (commonly via tags) to apply different baselines
- **Maintenance Windows**: control *when* patching happens
- **Compliance reporting**: see which instances are missing patches

**SAA exam angle**
- If you see “automated patching,” “patch compliance,” or “maintenance window,” think Patch Manager + Maintenance Windows.

---

## Maintenance Windows
A **Maintenance Window** schedules when actions are allowed to run:
- Patch installs
- Run Command tasks
- Automation tasks

**SAA exam angle**
- Use this when the scenario requires changes only during approved time windows (e.g., “Sundays 02:00–04:00”).

---

## State Manager (Desired State & Drift Control)
**State Manager** helps keep instances in a **desired configuration state** using **associations**:
- “Ensure this agent is installed”
- “Ensure this config file exists”
- “Ensure this service is running”

**SAA exam angle**
- If the prompt says “ensure all instances stay configured like X” or “enforce configuration consistently,” think State Manager.

---

## Parameter Store
**Parameter Store** stores configuration values centrally:
- Plain text parameters
- **SecureString** parameters (encrypted with KMS)
- Hierarchical paths (e.g., `/prod/app/db/url`)
- Versioning and access control via IAM

Useful for:
- Database connection strings, feature flags, endpoints
- Lightweight secret-like storage (though see note below)

**Important SAA nuance: Parameter Store vs Secrets Manager**
- Parameter Store can store encrypted values (SecureString) but **Secrets Manager** is purpose-built for secrets rotation and secret lifecycle features.
- If the question explicitly mentions **automatic rotation** of secrets (like RDS credentials), **Secrets Manager** is usually the right answer.

---

## Inventory
**SSM Inventory** collects metadata about managed instances and stores it centrally for query/reporting.

Common inventory items:
- OS name/version
- Installed applications (names/versions)
- Network configuration
- Windows updates, roles, services
- CPU details

**SAA exam angle**
- If the prompt asks for “visibility into installed software across fleet” or “collect inventory at scale,” Inventory is a fit.

---

## Ops & Insights (Explorer / OpsCenter)
Systems Manager includes operational views that aggregate data about your fleet:
- Compliance status (patching, associations)
- Operational events
- Helpful recommendations

(Exact naming/features evolve, but the exam pattern is usually: “centralized ops visibility” → SSM operational views.)

---

## Common “Prerequisites” Checklist (Exam Gold)

For an EC2 instance to be managed by SSM, you generally need:
1. **SSM Agent installed and running**
2. **IAM instance profile** granting SSM permissions (commonly `AmazonSSMManagedInstanceCore`)
3. **Network connectivity** to SSM endpoints (internet/NAT or VPC endpoints)
4. Optional but common:
   - KMS permissions if using encrypted parameters/logging
   - CloudWatch Logs permissions if logging sessions/commands to CloudWatch

For **on-prem / hybrid**:
- You register servers using a managed instance activation (hybrid activation) and ensure outbound connectivity.

---

## Compliance
SSM helps with compliance by providing:
- Patch compliance (who is missing critical updates)
- Association compliance (who drifted from desired state)
- Auditable access via Session Manager logs
- Centralized visibility across fleets

**SAA exam angle**
- Compliance + audit + “no inbound ports” often points toward SSM (especially Session Manager + Patch Manager).

---

## Quick “When to Use What” Cheat Sheet

- **Need fleet command execution, no SSH:** Run Command  
- **Need multi-step workflow / orchestration:** Automation (Runbooks)  
- **Need secure shell without inbound ports / bastion:** Session Manager  
- **Need scheduled patching + compliance:** Patch Manager + Maintenance Windows  
- **Need desired state config enforcement:** State Manager (Associations)  
- **Need config values/secrets-ish storage:** Parameter Store (SecureString + KMS)  
- **Need secret rotation:** Secrets Manager (not SSM)  
- **Need installed software inventory:** Inventory  
