---
sort: 4
---

# Jupyter Notebook

Jupyter Notebook is an interactive web application that provides an environment where you can create and share documents with live code, equations, visualizations, and narrative text. It is great for data analysis, scientific computing, and machine learning tasks - you can run Python code in cells, see results right away, and document your work all in one place.

## Running Jupyter Notebook

To start Jupyter Notebook on the cluster, you would use either the `sbatch` or `srun` commands. `sbatch` is typically used to run scripts or commands that can execute without user intervention. `srun` is used to launch applications in an interactive pseudo-terminal session. As the Jupyter Notebook server process does not require any interactivity itself, `sbatch` is sufficient. `sbatch` has the added benefit that you can set the options at the top of the job script to make its reuse more convenient.


### Setup

Before jumping in and running Jupyter Notebook, you may need to install required packages and stage your data.

Remember: The compute nodes do not have access the Internet themselves, so you need to transfer any files you need through the login node first.

Please see our guide on using conda and [how to transfer files]({{site.baseurl}}{% link storage/file_transfer.md %}).

```note
#### Using Your Storage Effectively

Usually, for most of your work you should store your files at `/fs1/projects/{project-name}/`, which lives on the parallel file-system storage. You can also use your home directory (`/home/{username}/`) for quick experiments and convenient  access to scripts, but keep in mind that your home directory has limited storage space and performance. The parallel file-system storable is much faster and has way more space for your notebooks and data.
```

### Job Script

You'll typically use a job script to launch Jupyter Notebook and most other applications after performing any initial setup. Below is an example that you can just copy and paste to get started. Save it as `jupyter.sbatch`:

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

```warning
#### Don't forget to replace the placeholders!

The words between <...> need to be replaced with what _you_ need. For instance:
- <compute-node> needs to be replaced with the node(s) available [here]({{site.baseurl}}{% link quickstart/about-star.md %}).
- <project-name>, or the entire output path, needs to be replaced with the directory _you_'d like to save the output/error files to.
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

Once Jupyter Notebook is running, you'll need to run one or more SSH commands to setup SSH port forwarding so you can access it.

### SSH Port Forwarding

As the compute nodes where workloads run on the cluster are not directly reachable from the campus network, you'll need to use SSH port forwarding through the login node to access your Jupyter Notebook instances on the cluster. Also, as the login node itself is not currently reachable off campus, either SSH port forwarding through the Linux lab machines or VPN access is needed to access the login node when off campus.

1. The job script (shown in the next section) will generate an SSH command in your output file
2. Run this command from your local machine to establish the connection through the Linux lab machine
3. Access Jupyter through your local web browser

## Working on the Same Node

Need to run commands on the node where Jupyter is running? Use `srun` to get an interactive shell:

```bash
srun --jobid=<your_jupyter_job_id> --pty bash
```

Check out [Interactive jobs]({{site.baseurl}}{% link jobs/submitting-jobs.md %}#interactive-jobs) for more details about interactive sessions.

## Using Existing Container Images
You can also run Docker images on the cluster through Apptainer (a variant of Singularity). This is great when you want an environment with everything pre-installed. Check out the [Apptainer Guide]({{site.baseurl}}{% link software/apptainer.md %}) to learn more.
