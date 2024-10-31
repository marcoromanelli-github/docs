---
sort: 4
---

# Job Scheduling and Resource Optimization

This guide aims to help users optimize their job submissions on the Star HPC cluster by providing an understanding of the scheduling system and resource allocation policies. By tailoring your job scripts and resource requests effectively, you can enhance job efficiency, reduce wait times, and make the most of the cluster’s capabilities.

## Why Is My Job Not Running Yet?

You can use the `squeue -j <jobid>` command to see the status and the reason why your job is not running. There are several factors which affect this such as your job’s priority, which is a function of fairshare, job age, partition, job size, etc. It may also be possible that your job is asking for more resources than allocated, in which case it will never even start. To pinpoint the exact issue with your job, let’s get more clarity on the factors at play here:

### 1. Scheduling Policies

Your job's priority might be lower due to factors like recent high resource usage (fairshare), the QoS selected, or the partition used.  

Let’s understand slurm scheduling in more detail:  
The cluster operates on a **Basic Multifactor Priority**, based on **First-In and First-Out scheduling**. Where the **fair-share hierarchy** represents the portion of the computing resources that have been allocated to different projects, these allocations are assigned to an account. Understanding the factors that influence job priority can help you strategize your job submissions for faster execution. Two key features of slurm are **Fairshare** and **Backfill**.

---

#### i) Fairshare

The more resources your jobs have already consumed within an account, the lower priority factor your future jobs will have when compared to other users' jobs in the same account who have used fewer resources (so as to "fair-share" with other users). Additionally, if there are two members of a given account, and if one of those users has run many jobs under that account, the job priority of a job submitted by the user who has not run any jobs will be negatively affected. This ensures that the combined usage charged to an account matches the portion of the machine that is allocated to that account.

The job's priority at any given time will be a weighted sum of all the factors that have been enabled in the cluster.  

- **Age**: the length of time a job has been waiting in the queue, eligible to be scheduled  
- **Association**: a factor associated with each association  
- **Fair-share**: the difference between the portion of the computing resource that has been promised and the amount of resources that has been consumed  
- **Job size**: the number of nodes or CPUs a job is allocated  
- **Partition**: a factor associated with each node partition  
- **Quality of Service (QOS)**: a factor associated with each Quality Of Service  
- **Site**: a factor dictated by an administrator or a site-developed job_submit or site_factor plugin  
- **TRES**: each TRES Type has its own factor for a job which represents the number of requested/allocated TRES Type in a given partition  

#### Command line examples:

1. **Displaying the sharing and Fair-Share information of your user in your account.**
   ```bash
   $ sshare -l -U -o Account,User,NormShares,RawUsage,NormUsage,EffectvUsage,FairShare,TRESRunMins%100
   ```

   **Sample Output:**
   ```text
   Account              User     NormShares  RawUsage NormUsage  EffectvUsage   FairShare                                        TRESRunMins 
   -----------------------------------------------------------------------------------------------------------------
   ResearchGroup1 <user>       0.000705       100942    0.000081       0.000003     0.997186   cpu=127851,mem=465054140,gres/gpu=27134
   ```

2. **Displaying the FairShare information of all users of your account**
   ```bash
   $ sshare -a --accounts=<account>
   ```

   **Sample Output:**
   ```text
   Account                        User      RawShares     NormShares    RawUsage  EffectvUsage    FairShare 
   ------------------------------------------------------------------------------------------------------------------------------------
   ResearchGroup1                                           1              0.023256           117684        0.000094      0.997199 
     ResearchGroup1    <user1>                        1              0.000705             16765       0.000003      0.997193 
     ResearchGroup1    <user2>                        1              0.000705           100918       0.000003      0.997187
   ```

3. **Displaying a summary of the six factors configured that comprise each job’s scheduling priority**

The sprio -w option displays the weights (PriorityWeightAge, PriorityWeightFairshare, etc.) for each factor as it is currently configured.

   ```bash
   $ sprio -w
   ```

   **Sample Output:**
   ```text
   JOBID   PARTITION   PRIORITY   SITE    FAIRSHARE   JOBSIZE   PARTITION   QOS
   ---------------------------------------------------------------------------------------------------------------------
   Weights                                                1               10000          1000              1000    1000
   ```

---

#### ii) Backfilling

Backfilling is a technique to optimize resource utilization. If a large job is waiting for specific resources, the scheduler allows smaller jobs to run in the meantime, provided they don't delay the start of the higher-priority job. This approach keeps the cluster busy and reduces idle time.

---

### 2. Resource Availability

The required resources may not be available at the moment. Jobs might have to wait longer for sufficient resources to free up. Resources are assigned to accounts based on **QOS (Quality of Service).**

---

## What Are Accounts?

Users can belong to multiple Slurm accounts and multiple users can belong to a single Slurm account. Slurm accounts typically correspond with a group of users and we generally create them on a project basis. Slurm accounts are used for tracking usage and enforcing resource limits. When new project accounts are created, they will consume a portion of the shares allocated for the parent account, which is typically associated with a broader administrative entity, such as an institution, department, or research group.

---

## What QOS Should I Choose for My Job?

QoS (Quality of Service) define job with different priorities and resource limits. Selecting the appropriate QoS can influence your job’s priority in the queue. Be mindful of the tradeoff that comes with a higher QOS: while higher QoS levels allow longer runtimes, they may result in longer wait times due to lower scheduling priority.

- **short**: For jobs up to 1 hour, with higher priority, suitable for testing and quick tasks.  
- **medium**: For jobs up to 48 hours, balanced priority for standard workloads.  
- **long**: For jobs up to 120 hours, lower priority due to resource demands, suitable for extensive computations.  

---

## When Will My Job Start?

While exact start times can't be guaranteed due to the dynamic nature of the cluster workload, you can get an estimate:

```bash
squeue -j <jobid> --start
```

You can look at the Slurm queue to get a sense for how many other jobs are pending in the relevant partition and where your job is in the queue:

```bash
squeue -p <partition_name> --state=PD -l
```

To see the jobs run by other users in your group, specify the account name:

```bash
squeue -A <account>
```
Review your fairshare score using sshare to understand how your recent resource usage might be affecting your job's priority.


---

## Can I Have More Resources?

It depends. We don’t have unlimited resources, so please try to make the most of the resources available. Moreover, it is quite possible that you are not using the resources that you have completely. Using your current resources completely may suffice your needs.

Before requesting additional resources, make sure you are optimally using the resources you have already been allocated. To request an additional allocation, provide a brief justification, which may include how you are using your current allocation.

---

## How Can I Make Sure That I Am Using My Resources Optimally?

You can tailor your job according to its duration and the resources it needs.

When submitting a **short job**, consider using the **short QoS** to gain higher priority in the queue. Request only the necessary resources to keep the job lightweight and reduce wait times.

**Example:**

```bash
#!/bin/bash
#SBATCH --job-name=debug_job
#SBATCH --partition=mixed-use
#SBATCH --qos=short
#SBATCH --time=00:30:00
#SBATCH --mem=1G

module load python3
python3 quick_task.py
```
---

## **How Can I Submit a Long Job?**

For long jobs, select the **long QoS**, which allows for extended runtimes but may have lower scheduling priority. It’s advisable to implement **checkpointing** in your application if possible. Checkpointing allows your job to save progress at intervals, so you can resume from the last checkpoint in case of interruptions, mitigating the risk of resource wastage due to unexpected failures.

Be aware of **fairshare implications**; consistently running long jobs can reduce your priority over time. Plan your submissions accordingly to balance resource usage.

**Example:**

```bash
#!/bin/bash
#SBATCH --job-name=long_job
#SBATCH --partition=gpu-large
#SBATCH --qos=long
#SBATCH --time=120:00:00
#SBATCH --nodes=2
#SBATCH --mem=64G

srun python train_model.py
```

This script requests sufficient time and resources for an extended computation, using the **long QoS**.

---

## **What Can I Do to Get My Job Started More Quickly? Any Other PRO Tips?**

1. **Shorten the time limit** on your job, if possible. This may allow the scheduler to fit your job into a time window while it is trying to make room for a larger job (using Slurm's **backfill functionality**).

2. **Request fewer nodes** (or fewer cores on partitions scheduled by core), if possible. This may also allow the scheduler to fit your job into a time window while it is waiting to make room for larger jobs.

3. **Resource Estimation**:  
   Monitor the resource usage of your previous jobs to inform future resource requests. Use tools like `sacct` to review past job statistics:

   ```bash
   sacct
   ```

4. **Efficient Job Scripts**:  
   Simplify your job scripts by removing unnecessary module loads and commands. This reduces overhead and potential points of failure.

5. **Implement Checkpointing**:  
   For long-running jobs, incorporate checkpointing to save progress at intervals. This allows you to resume computations without starting over in case of interruptions.

6. **Avoid Over-Requesting Resources**:  
   Requesting more CPUs, memory, or time than needed can increase your job’s wait time and negatively impact **fairshare calculations**.

7. **Understand Scheduling Policies**:  
   Familiarize yourself with the cluster’s **scheduling policies**, including **fairshare** and **backfilling**. This knowledge can help you strategize your job submissions for better priority.

8. **Communicate with Your Group**:  
   If you’re part of a research group, coordinate resource usage to avoid collectively lowering your group’s **fairshare priority**.

---
