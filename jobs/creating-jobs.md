---
sort: 2
---

# Creating Jobs
## Batch jobs (Non-interactive)
Batch jobs allow users to execute tasks without direct interaction with the computing environment. These jobs are written in scripts, which include commands for the job's execution and resource requests which will be interpreted by the cluster scheduler which is Slurm in our case, and are submitted to the job scheduling system.
### BATCH directives
BATCH directives are essentially instructions embedded at the beginning of a batch job script and are interpreted by the scheduler (like Slurm in our case). These lines are prefixed with `#SBATCH` for Slurm and inform the scheduler about the resources needed for the job and any other execution preferences.

Here are a list of common directives: <br>
* `#SBATCH --nodes=1`: Requests a specific number of nodes.
* `#SBATCH --ntasks-per-node=1`: Defines the number of tasks to run on each node.
* `#SBATCH --mem=4G`: Specifies the amount of memory required.
* `#SBATCH --time=01:00:00`: Sets the maximum runtime (hh:mm:ss).
* `#SBATCH --partition=standard`: Specifies the queue/partition where the job should be submitted.
* `#SBATCH --output=result.txt`: Directs the job's output to a specific file. <br><br>
**Note:** These bullets are just for a better basic understanding on the topic. Complete examples and line-by-line explanations are provided further down on this page.

### Queues and partitions
Queues (or partitions in Slurm terminology) are categories within the cluster that organize jobs based on their resource requirements, priority, and other factors.

Our cluster is partitioned into the following categories: <br>
* **Standard Partition (future_parition_name)**: For general-purpose jobs with moderate resource requirements.
* **High-Memory Partition (future_parition_name)**: For jobs requiring significant amounts of memory.
* **GPU Partition (future-partition_name)**: For jobs that need GPU resources, such as gpu1 and gpu2 in our setup.
### Simple batch job example
### Advanced batch job example

