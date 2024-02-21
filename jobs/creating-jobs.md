---
sort: 2
---

# Creating Jobs
## Batch jobs (Non-interactive)
Batch jobs allow users to execute tasks without direct interaction with the computing environment. These jobs are written in scripts, which include commands for the job's execution and resource requests which will be interpreted by the cluster scheduler which is Slurm in our case, and are submitted to the job scheduling system.
### BATCH directives
BATCH directives are special comments at the top of a batch job script that provide the scheduler (like Slurm) with instructions about how to handle the job. These include resource requests (such as the number of nodes, CPUs per task, amount of memory, and job runtime) and other job-related settings. We will introduce some common and more advanced directives in this page's example scripts.
### Queues and partitions
### Simple batch job example
### Advanced batch job example

