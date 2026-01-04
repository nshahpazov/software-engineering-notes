# Some basic comparison of different services for processing


- Glue is specialized for data engineering & data lakes, not generic batch compute.
- AWS Batch, on the other hand, is "Run arbitrary containerized batch jobs at scale, with queues, retries and autoscaling.”"



### Example use cases
* We have data in S3 (maybe JSON/CSV/parquet) and need to:
- clean / transform / join / aggregate
- write back to S3 as optimized Parquet/Iceberg
- and expose it to Athena / Redshift / EMR”

Usually wired with Glue Crawlers, Data Catalog, and Athena.

Use AWS Batch when your story sounds like:
“We have jobs, not just ETL**. They’re heavy, often parallel, maybe long-running, and each runs in a container we control completely. Some are ETL, some aren’t.”

1. HPC / scientific stuff
- Thousands of simulation jobs
- Monte Carlo risk analysis
- Genomics pipelines.
2. Big, custom ETL not well-suited to Glue
- ETL that uses non-Spark tools, custom C++ binaries, weird system libs, etc.
- Jobs where you need fine-grained control of CPU/memory/GPU per container.
3. ML model training at scale
- Training jobs that need multiple GPUs
- Hyperparameter tuning with many parallel training jobs
4. Media processing at scale
- Large-scale video transcoding
- Image rendering farms