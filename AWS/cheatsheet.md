## 🧠 AWS Multi-Region DR Cheat Sheet  
**Services:** RDS vs Redshift vs DynamoDB

---

### 🔤 Common Terms (for context)

- **RTO** – Recovery Time Objective → *How long can we be down?*  
- **RPO** – Recovery Point Objective → *How much data can we lose?*  

---

## 💾 Amazon RDS (MySQL/Postgres/etc., NOT Aurora)

### ✅ Main Multi-Region DR Options

| Pattern | What it is | RTO/RPO (rough) | Notes / Exam Clues |
|--------|------------|-----------------|---------------------|
| **Cross-Region Read Replica** | Async replica in another region | Low–medium RTO, low RPO | Promote to standalone DB during disaster. Good for DR + read offloading. |
| **Cross-Region Snapshot Copy** | Snapshots automatically copied to another region | Higher RTO, higher RPO | Cheap DR, slower recovery. Suitable if some downtime/data loss is acceptable. |

### 🚫 What it is NOT

- **Multi-AZ** = high availability **within** a region, **not** multi-region DR.
- One RDS instance cannot be “stretched” across regions.

### 📝 Exam Patterns

- “**Region outage** + RDS + low downtime” → **Cross-region read replica + promote on failover**.  
- “**Cost-effective DR** + OK with downtime” → **Cross-region snapshot copy**.  
- If they say **Aurora**, that’s slightly different (Aurora Global Database).

---

## 🌐 Amazon Aurora (RDS family but special snowflake)

Worth including because it often shows up alongside RDS:

| Feature | Use | Clue |
|--------|-----|------|
| **Aurora Global Database** | One primary region, up to 5 secondary regions; low-latency global reads; fast DR | “Global latency”, “multi-region DR with Aurora”, “sub-minute RPO, low RTO” |
| **Cross-Region Read Replicas** | Older pattern, still valid | Less fancy than Global DB |

---

## 📊 Amazon Redshift

### ✅ Main Multi-Region DR Option

| Pattern | What it is | RTO/RPO | Notes |
|--------|------------|---------|-------|
| **Cross-Region Snapshot Copy** | Redshift automatically copies snapshots to another region | Medium/high RTO, medium RPO | Spin up new cluster in DR region from copied snapshot. |

### 🚫 Wrong Answers / Traps

- “Do nothing, Redshift is highly available” → **WRONG** for region outage.  
- “Just automated snapshots” → Only protects **within same region** unless cross-region copy is enabled.  
- “Custom script copying snapshots to S3 in another region” → Overkill when **cross-region snapshot copy** exists.

### 📝 Exam Patterns

- **“Redshift + disaster recovery + region outage” ⇒ `Enable Cross-Region Snapshot Copy`**.  
- Anything implying Redshift magically survives a region dying is nonsense.

---

## 📚 Amazon DynamoDB

### ✅ Main Multi-Region Patterns

| Pattern | What it is | RTO/RPO | Notes / Use Cases |
|--------|------------|---------|-------------------|
| **DynamoDB Global Tables** | Multi-region, multi-active replication | Very low RTO & RPO | Writes can happen in multiple regions; built-in async replication, good for global apps and DR. |
| **Backup & Restore to another Region** | On-demand or PITR backup, restore into a table in another region | Higher RTO/RPO | Cheap DR if you don’t need active-active or tight RPO. |

### 📝 Exam Patterns

- “**Global, multi-region, active-active NoSQL**” → **Global Tables** almost every time.  
- “**DR only, cost-sensitive, OK with downtime**” → **backup + restore in another region**.  
- If they mention **streams + Lambda replication**, that’s the old DIY pattern; now **Global Tables** is preferred.

---

## 🧾 Quick Comparison Table

| Service    | Best Native Multi-Region DR Feature        | Active-Active? | Typical Exam Answer Wording |
|-----------|--------------------------------------------|----------------|-----------------------------|
| **RDS**   | Cross-Region **Read Replicas** or **Snapshot Copy** | No (replica is read-only until promoted) | “Create cross-region read replica and promote during outage” |
| **Aurora**| **Aurora Global Database**                 | Reads: yes, writes: primary region only | “Use Aurora Global Database for low-latency global reads + DR” |
| **Redshift** | **Cross-Region Snapshot Copy**          | No             | “Enable cross-region snapshot copy for DR” |
| **DynamoDB** | **Global Tables**                       | Yes            | “Use DynamoDB Global Tables for multi-region, active-active DR” |
