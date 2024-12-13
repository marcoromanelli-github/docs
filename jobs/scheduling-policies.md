---
sort: 5
---

# Scheduler Policies (Alex's version)

## Job Priority

The job scheduler (Slurm) on Star uses a priority based scheduling method. Each job submitted is assigned a priority in order to determine the relative importance and the order in which to schedule the pending jobs. A job's priority is an integer value that is calculated based on a number of factors as explained below. Jobs start when sufficient resources (CPUs, GPUs, memory, licenses) are available and are not already reserved for a job with higher priority. A job's priority determines its position in the queue relative to other jobs and the order in which the pending jobs will run. The pending job with the highest priority will, in principle, be scheduled first, except when a smaller/shorter job can start without delaying a job with a higher priority, a strategy known as backfilling. There are several commands that can be used to to get insights in why your job is waiting and what the position of your job is in the queue. These commands can be found on this page as well.

## Priority Factors

Job priority scores are determined by a number of factors:

### Quality of Service (QoS)

The QOS factor is a value given by the QoS associated with the job, specified at job jubmission with the `--qos` option.

The "debug" Quality of Service can be gained by adding the sbatch command line option `--qos=debug`.
This adds 5000 to the job priority so raises it above all non-debug jobs, but is limited to one small job per user at a time: no more than 15 minutes and no more than 2 nodes.

### Fair Share

We use Slurm's concept of "fair-share" to promote balanced resource usage among accounts. The fair-share factor is designed such that the scheduler deprioritizes accounts with excessive resource utilization. It makes sure that accounts that have not used the cluster as much get a higher priority for their jobs, while accounts that have used the cluster a lot don't overuse it.

The job priority decreases whenever the project uses more core-hours than expected. The [Fair Share]({{site.baseurl}}{% link jobs/fairshare.md %}) policy means that projects that have consumed many CPU core hours in the recent past compared to their expected rate of use (either by submitting and running many jobs, or by submitting and running large jobs) will have a lower priority, and projects with little recent activity compared to their expected rate of use will see their waiting jobs start sooner.

The fair-share factor is a fractional number between 0 and 1 that is assigned to all accounts based on their past usage. Slurm computes this number regularly and it changes based on your usage and on the total number of accounts on the system. The job priority calculation considers this variable in determining the priority of pending jobs. On Star, the Fair Share factor can contribute up to 1000 points to the job priority. To see the recent usage and current fair-share score of a project, you can use the command nn_corehour_usage.

The faishare parameter has a 'forgetting' threshold that causes it to only consider the recent history of the account and not the account's total use throughout its lifetime.

### Job Age

Job priority slowly rises with time as a pending job gets older -1 point per hour for up to 3 weeks.

Note that the job age parameter is bounded so that priority stops increasing when the bound is reached.

### Job Size or "TRES" (Trackable RESources)

The job size factor can be configured to favor small or large jobs. Currently on Star, this factor is configured to prioritize smaller jobs. This factor is also often used to favor jobs that request a larger count of CPUs (or memory or GPUs) as a means of countering their otherwise inherently longer wait times.

### Project Allocation Class

| Project class | Class Priority Score |
|---------------|----------------------|
| Proposal Development | 10 |
| Postgraduate         | 20 |
| Collaborator         | 30 |
| Merit                | 40 |
| Commercial           | 40 |

### Nice values

It is possible to give a job a "nice" value which is subtracted from its priority. You can do that with the `--nice` option of `sbatch` or the `scontrol update` command.  The command `scontrol top <jobid>` adjusts nice values to increase the priority of one of your jobs at the expense of any others you have in the same partition.

### Holds

Jobs with a priority of 0 are in a "held" state and will never start without further intervention.  You can hold jobs with the command `scontrol hold <jobid>` and release them with `scontrol release <jobid>`.  Jobs can also end up in this state when they get requeued after a node failure.

## Other Limits

Cluster and partition-specific limits can sometimes prevent jobs from starting regardless of their priority score.  For details see the [partition limits](#) page.

## Priority Calculation

Slurm calculate the priority of each job as a weighted sum of these factors:

```
Job_priority =
    (PriorityWeightAge)       * (age_factor)        +
    (PriorityWeightFairshare) * (fair-share_factor) +
    (PriorityWeightJobSize)   * (job_size_factor)   +
    (PriorityWeightPartition) * (partition_factor)  +
    (PriorityWeightQOS)       * (QOS_factor)        +
    (possibly some other advanced factors that are not relevant for Star)
```

All the factors in this formula are floating point numbers between 0.0 and 1.0, while the weights are integer values that determine how important the factors should be considered.

The current configuration of the cluster can be found by running the following command:

```
scontrol show config | grep ^Priority
```

As of this writing, we use the following weights in the Slurm configuration on Star:

(TBD)

This means that the priority of a job is mainly determined by a fairshare component and a little bit by its age.
The age of a job refers to how long a job has already been waiting in the queue on a time scale from 0 to 100 days: if it was just queued, the age factor will be 0.0. After 50 days of waiting the age factor will be 0.5 and after 100 days or more the job’s age factor will be and stay at the maximum value of 1.0.
Finally, the most important factor is the fairshare factor: it indicates how much a user recently has been using the system compared to the share of the system that was allocated to this user. This usage decays over time and favors the most recent usage statistics: assuming the user would not use the cluster anymore, his or her usage will decay to half of its original value after a configured half-life period of one week.

## Backfill

Backfill is a scheduling strategy in which lower priority jobs can be scheduled earlier than higher priority jobs to fill idle slots, provided they are finished before the next high priority job is expected to start based on resource availability. In other words, backfilling allows small, short jobs to run immediately if in doing so they will not delay the expected start time of any higher-priority jobs. Since the expected start time of pending jobs depends upon the expected completion time of running jobs it is important that all users set reasonably accurate job time limits for backfilling to work well.

While the kinds of jobs that can be backfilled will also get a low job size score, it is our general experience that an ability to be backfilled is on the whole more useful when it comes to getting work done on the cluster.

More information about backfilling can be found at [SchedMD's Scheduling Configuration Guide](https://slurm.schedmd.com/sched_config.html){:target="_blank"}.

## Useful commands related to priority and fairshare

### sprio: show priority per job

The [sprio](http://slurm.schedmd.com/sprio.html){:target="_blank"} command shows the priority per job, including the individual components for the job age and fairshare (both already multiplied by their corresponding weights). This can be useful for comparing jobs to other waiting jobs and finding out why a job is still waiting.

### sshare: show your fairshare number

For a user running the [sshare](http://slurm.schedmd.com/sshare.html){:target="_blank"} command, it will show in more detail the current fairshare number for this user and the two main components that determine this number: the (normalized) share of the system that was assigned to him/her and the effective usage of the system by this user.

### squeue: show all jobs, priorities, estimated start times

The [squeue](http://slurm.schedmd.com/squeue.html){:target="_blank"} command shows all jobs on the system and sorts them, by default, by status and priority: first the waiting jobs are shown by descending priority, then the running ones. The position in the queue could give some kind of indication about when your job will start. In order to list the actual priorities, one can run squeue with some additional flags:

```
squeue -o "%.18i %.9P %.8j %.8u %.2t %.10M %.6D %.18R %p"
```

This will include all the default columns and, additionally, a column with the actual priority of each job.

Another useful option for squeue is `--start`: it will show the estimated start time for (some) waiting jobs, in case SLURM can already calculate one. Note that these are very rough estimates, since they depend on several factors.

The priority given to a job can also be obtained with squeue:

```
squeue -o %Q -j jobid
```

## Sources

* https://wiki.hpc.rug.nl/habrok/advanced_job_management/job_prioritization
* https://docs.nesi.org.nz/Scientific_Computing/Running_Jobs_on_Maui_and_Mahuika/Job_prioritisation/
