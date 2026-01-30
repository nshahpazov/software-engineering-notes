## Instance Types

### General Purpose (T Series, M Series)
- **t3, t4g**: Burstable performance instances, suitable for workloads that don't use the full CPU often.
- **m5, m6g**: Balanced compute, memory, and networking resources for a wide range of applications.

Good for a variety of diverse workloads. If you are unsure which instance type to choose, start with general purpose instances.
They provide a balance of compute, memory, and networking resources, and can be used for a variety of workloads.

##### Examples
- Web and application servers
- Small and medium databases
- Development and test environments
- Backend servers for enterprise applications


### Compute Optimized (C Series)

Compute optimized instances are rich on CPU resources instead of memory. They are well suited for compute-bound applications that benefit from high-performance processors.
They are ideal for compute-bound applications that benefit from high-performance processors, such as batch processing workloads and media transcoding.



##### Examples
- Batch processing
- Media transcoding
- High-performance web servers
- Scientific modeling
- Dedicated gaming servers
- Ad serving
- CPU-intensive ML inference

### Memory Optimized (R Series, X Series, U Series)

Those instances are a lot more heavy on RAM compared to CPU. They are well suited for memory-intensive applications.
Memory Optimized Instances are designed to deliver fast performance for workloads that process large data sets in memory, which is quite different from handling high read and write capacity on local storage.



##### Examples
- High-performance in-memory databases
- Real-time big data analytics
- SAP HANA

##### Note
If you have a requirement like "in memory cache", "in-memory database", "real-time big data analytics", you should probably consider memory optimized instances.

### Storage Optimized (D Series, I Series)

Optimized for local NVMe SSD with a lot of IOPS and throughput. They are well suited for workloads that require high, sequential read and write access to very large data sets on local storage. They are optimized to deliver tens of thousands of low-latency, random I/O operations per second (IOPS) to applications.

##### Examples
- NoSQL databases like Cassandra and MongoDB
- Data warehousing applications and real-time analytics
- Elasticsearch
- Hadoop distributed computing
- Log or data processing applications

##### Note
> "high, sequential or random I/O to local storage”, “millions of small reads/writes”, “needs low disk latency”.

If you have requirements like "very high IOPS", "low-latency access to local SSD", "high throughput to local storage", "locally attached NVM SSD" you should probably consider storage optimized instances.


### Accelerated Computing
> DL1 | DL2q | F1 | F2 | G4ad | G4dn | G5 | G5g | G6 | G6e | G6f | Gr6 | Gr6f | Inf1 | Inf2 | P4d | P4de | P5 | P5e | P5en | P6-B200 | P6-B300 | P6e-GB200 | Trn1 | Trn1n | Trn2 | Trn2u | VT1


##### Examples
- Machine learning training and inference
- High-performance computing (HPC)
- Graphics rendering
- Video transcoding

##### Note
> "GPU", "CUDA", "machine learning training", "machine learning inference", "graphics rendering", "video transcoding".

### High Performance Computing (HPC)

Good for tightly coupled high performance computing (HPC) applications that require high levels of inter-node communications bandwidth, low latency networking, and/or high levels of IOPS.

##### Examples
Computational Fluid Dynamics (CFD)
Weather simulations
Molecular dynamics
Finite element / multiphysics simulations
All kind of MPI based workloads


## Additional Notes

### EFA (Elastic Fabric Adapter)
Elastic Fabric Adapter (EFA) is basically AWS’s special high-speed network card for EC2, designed so that many instances can talk to each other with very low latency and high throughput, especially for HPC and tightly-coupled workloads.

### What EFA actually is
- It’s a network interface (like an ENI) that you can attach to supported EC2 instances.
- It lets instances in a cluster communicate using OS-bypass networking (via libfabric / MPI), which avoids a bunch of kernel overhead.
- Result: lower latency, higher and more consistent throughput, and much better scalability when many nodes are chatting all the time.