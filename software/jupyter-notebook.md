---
sort: 4
---

# Jupyter Notebook

Jupyter Notebook is an interactive web application that provides an environment where you can create and share documents with live code, equations, visualizations, and narrative text. It is great for data analysis, scientific computing, and machine learning tasks - you can run Python code in cells, see results right away, and document your work all in one place.

## Storage Space and Performance

While you can use `/home/<username>/` for quick experiments and individual projects, keep in mind that this path has limited storage space and performance. For most of your work, use `/fs1/projects/<project-name>/` which lives on the parallel file-system storage. It's faster and has much more space for your notebooks and data.

## SSH Port Forwarding

As the compute nodes where workloads run on the cluster are not directly reachable from the campus network, you'll need to use SSH port forwarding through the login node to access your Jupyter Notebook instances on the cluster. Also, as the login node itself is not currently reachable off campus, either SSH port forwarding through the Linux lab machines or VPN access is needed to access the login node when off campus.

1. The job script (shown in the next section) will generate an SSH command in your output file
2. Run this command from your local machine to establish the connection through the Linux lab machine
3. Access Jupyter through your local web browser

## Running Jupyter Notebook

Here's a script to launch Jupyter. Save it as `jupyter.sbatch`:

```bash
#!/bin/bash

#SBATCH --nodelist=<compute-node>
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --time=00:30:00
#SBATCH --job-name=jupyter_notebook
#SBATCH --output=/fs1/projects/<project-name>/jupyter_%j.out
#SBATCH --error=/fs1/projects/<project-name>/jupyter_%j.err

XDG_RUNTIME_DIR=""
node=$(hostname -s)
user=$(whoami)
port=9001

echo -e "
Run this command from your local machine to set up the tunnel:
ssh -L ${port}:localhost:${port} -p 5010 ${user}@adams204xx.hofstra.edu ssh -L ${port}:${node}:${port} ${user}@<login-node>

Replace 'xx' with a number between 01-30 to select a Linux lab machine.
"

module load jupyter

jupyter notebook --no-browser --port=${port} --ip=${node}
```

The script uses these SLURM settings:
- `--nodelist`: Picks which compute node to use
- `--ntasks=1`: Runs one instance of Jupyter
- `--cpus-per-task=1`: Uses one CPU
- `--time=00:30:00`: Runs for up to 30 minutes

To get started:
1. Submit the job: `sbatch jupyter.sbatch`
2. Look in your output file (`jupyter_<jobid>.out`) for the SSH tunnel command
3. Run that command from your local machine, replacing the `xx` placeholder with a number between 01-30
4. Find the Jupyter URL with token in your error file (`jupyter_<jobid>.err`)
5. Open that URL in your local computer's browser

Remember: The compute nodes can't access the internet, so transfer any files you need through the login node first.

## Working on the Same Node

Need to run commands on the node where Jupyter is running? Use `srun` to get an interactive shell:

```bash
srun --jobid=<your_jupyter_job_id> --pty bash
```

Check out [Interactive jobs]({{site.baseurl}}{% link jobs/submitting-jobs.md %}#interactive-jobs) for more details about interactive sessions.

## Adding More Packages

### Container Images
You can also run Docker images on the cluster through Apptainer (a variant of Singularity). This is great when you want an environment with everything pre-installed. Check out the [Apptainer Guide]({{site.baseurl}}{% link software/apptainer.md %}) to learn more.
