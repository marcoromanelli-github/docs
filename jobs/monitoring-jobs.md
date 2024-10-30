---
sort: 4
---

# Monitoring Jobs

Here you can see how to manage and monitor your jobs on our HPC cluster. Whether you're running batch jobs, interactive sessions, or array jobs, these tools and commands will help you keep track of your work and manage your resources.

## Checking Job Status

### Using `squeue`

The `squeue` command is possibly your most common tool for viewing the status of jobs in the queue. Here's a basic usage:

```bash
squeue
```

Sample output:

```bash
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
   1234     batch  my_job    jsmith  R       5:23      1 cn01
   1235     batch array_job  jdoe    R       2:45      1 cn02
   1236       gpu  gpu_task  asmith  PD       0:00      1 (Resources)
```

To see **only** your job:

```bash
squeue -u your_username
```

To see jobs on a specific partition:

```bash
squeue -p partition_name
```

### Jobs States

These are common job states that you might see under the `ST` column of `squeue`'s output:

- R: Running
- PD: Pending
- CG: Completing
- CD: Completed
- F: Failed
- TO: Timeout
- CA: Cancelled

## Detailed Job Information

### Using `scontrol`

To get detailed informatnio about a specific job:

```bash
scontrol show job job_id
```

Sample output:

```bash
JobId=1234 JobName=my_job
   UserId=jsmith(1001) GroupId=users(1001) MCS_label=N/A
   Priority=4294901758 Nice=0 Account=default QOS=normal
   JobState=RUNNING Reason=None Dependency=(null)
   Requeue=1 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:0
   RunTime=00:10:12 TimeLimit=01:00:00 TimeMin=N/A
   SubmitTime=2023-06-01T10:00:00 EligibleTime=2023-06-01T10:00:00
   AccrueTime=2023-06-01T10:00:00
   StartTime=2023-06-01T10:05:00 EndTime=2023-06-01T11:05:00 Deadline=N/A
   PreemptEligibleTime=2023-06-01T10:05:00 PreemptTime=None
   SuspendTime=None SecsPreSuspend=0 LastSchedEval=2023-06-01T10:05:00
   Partition=batch AllocNode:Sid=login01:12345
   ReqNodeList=(null) ExcNodeList=(null)
   NodeList=cn01
   BatchHost=cn01
   NumNodes=1 NumCPUs=1 NumTasks=1 CPUs/Task=1 ReqB:S:C:T=0:0:*:*
   TRES=cpu=1,mem=4G,node=1,billing=1
   Socks/Node=* NtasksPerN:B:S:C=0:0:*:* CoreSpec=*
   MinCPUsNode=1 MinMemoryNode=4G MinTmpDiskNode=0
   Features=(null) DelayBoot=00:00:00
   OverSubscribe=OK Contiguous=0 Licenses=(null) Network=(null)
   Command=/home/jsmith/my_script.sh
   WorkDir=/home/jsmith
   StdErr=/home/jsmith/my_job.err
   StdIn=/dev/null
   StdOut=/home/jsmith/my_job.out
   Power=
```

### Cancelling Jobs

To cancel a job:

```bash
scancel job_id
```

To cancel all your jobs:

```bash
scancel -u your_username
```

To cancel all your pending jobs:

```bash
scancel -t PENDING -u your_username
```

## Modifying Jobs

If you initially submit a job and then remember some attribute needs to be changed, you don't need to cacnel and resubmit the whole job. You can modify certain attributes of a job that's already in the queue using the `scontrol update` command.

For example, to change the time limit of a job:

```bash
scontrol update JobId=job_id TimeLimit=2:00:00
```

To change the number of CPUs:

```bash
scontrol update JobId=job_id NumCPUs=4
```

## Monitoring Resource Usage

### Using `sstat`

For running jobs, you can use `sstat` to get resource usage statistics:

```bash
sstat -j job_id --format=JobID,AveCPU,AveRSS,AveVMSize
```

Sample output:

```bash
JobID     AveCPU        AveRSS        AveVMSize
-------- ------------ ------------ -----------
1234.0      00:05:23      1234K         4567K
```

### Using `sacct`

For completed jobs, use `sacct` o view accounting data:

```bash
sacct -j job_id --format=JobID,JobName,MaxRSS,Elapsed
```

Sample output:

```bash
JobID    JobName     MaxRSS    Elapsed
------------ ---------- ---------- ----------
1234         my_job        4096K   00:15:23
```

## Monitoring Cluster Status

### Using `sinfo`

To see the overall status of the cluster:

```bash
sinfo
```

Sample output:

```bash
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
defq*        up   infinite      1    mix gpu1
defq*        up   infinite      2   idle cn01,gpu2
```

To see more detailed node information on for example `gpu1`:

```bash
sinfo -n gpu1 -o "%n %c %m %t %f %G %D %P %C %O"
```

Sample output:

```bash
HOSTNAMES CPUS MEMORY STATE AVAIL_FEATURES GRES NODES PARTITION CPUS(A/I/O/T) CPU_LOAD
gpu1 128 1 mix location=local (null) 1 defq* 5/123/0/128 1.67
```

## Job Arrays

For job arrays, you can use most of the above commands with some modifications.

To see the status of all tasks in a job array:

```bash
squeue -j array_job_id
```

Sample output:

```bash
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
1234_1     batch array_job    jdoe  R       5:23      1 cn01
1234_2     batch array_job    jdoe  R       5:23      1 cn02
1234_3     batch array_job    jdoe PD       0:00      1 (Resources)
```

## Troubleshooting

If a job fails, try checking the following:

1. Look at the job's output and error files.
2. Check the job's resource usage with `sacct`
3. Verify that you requested sufficient resources, and your job did not get terminated due to needing more resources than requested.

Remember, if you're having persistent issues, don't hesitate to reach out to the support team.
