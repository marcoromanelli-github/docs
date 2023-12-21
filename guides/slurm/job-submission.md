# Submitting Jobs

In High-Performance Computing (HPC) environments, jobs are submitted to a job scheduler for dispatch and execution on the cluster. Jobs are typically non-interactive and may be queued for batch processing, based on demand and resource availability.

This guide provides the basic steps to submit a job to the cluster and monitor its status using Slurm.

## 1. Creating a Job Script

A job script is a shell script containing directives and commands that tell the job scheduler (like SLURM) how to run your job. It typically includes:

- **Resource Specifications**: Indicate the resources needed (like the number of nodes, CPUs per node, memory, and runtime).
- **Environment Setup**: Load necessary modules or set environment variables.
- **Execution Commands**: The actual commands to run your job.

Here's an example job script (named `example_job.sh`):

```bash
#!/bin/bash

#SBATCH --job-name=my_test_job
#SBATCH --output=/path/to/output/file/result.out
#SBATCH --error=/path/to/error/file/result.err
#SBATCH --ntasks=1
#SBATCH --mem=8000
#SBATCH --mem-per-cpu=1000
#SBATCH --time=10:00

module load python/3.8
python my_script.py
```

Explanation:
- `#!/bin/bash`: This line indicates that the script should be run in the bash shell.
- `#SBATCH --job-name`: Sets the name of the job.
- `#SBATCH --output`: Defines the names of the standard output for your job at your desired path.
- `#SBATCH --error`: Defines error log files for your job at your desired path.
- `#SBATCH --ntasks`: Number of tasks or processes. In this case, it's a single-task job.
- `#SBATCH --mem`: Total memory in megabytes (here, 8000MB or 8GB).
- `#SBATCH --time`: The maximum time for the job (here, 10 minutes).
- `#SBATCH --mem-per-cpu`: Memory per CPU in megabytes (here, 1000MB or 1GB).
- `module load python/3.8`: Loads the Python module.
- `python my_script.py`: The command to run your Python script.

You should adjust the resource specifications based on your job's requirements and policies of the cluster.

**Modules and Environment**: The environment setup in the script, e.g. via `module load`, depends on the software and modules available on the cluster.

## 2. Submitting the Job using `sbatch`

To submit the job, use the `sbatch` command:

```bash
sbatch example_job.sh
```

This command sends your job script to the SLURM scheduler, which will queue it for execution based on the available resources and your script's resource requirements.

## 3. Monitoring the Job

After submitting, you can monitor your job's status and view your job queue.

- **Check Job Status**: Use the `squeue` command to see all running and queued jobs. To see only your jobs:

  ```bash
  squeue -u your_username
  ```

- **View Job Information**: To get more detailed information about a specific job, use:

  ```bash
  scontrol show job your_job_id
  ```

- **Canceling a Job**: If you need to cancel a job, use:

  ```bash
  scancel your_job_id
  ```

- **Checking Output**: The output of your job (if any) will be written to the file specified in the `--output` directive of your script (in this case, `result.txt`).


