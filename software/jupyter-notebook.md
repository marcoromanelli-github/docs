---
sort: 4
---

# Jupyter Notebook

Jupyter Notebook is an interactive web application that provides an environment where you can create and share documents with live code, equations, visualizations, and narrative text. It is great for data analysis, scientific computing, and machine learning tasks. You can run Python code in cells, see results right away, and document your work all in one place.

```note
This example uses local storage as we are not dealing with large amounts of data. If you need to import and use large datasets, make sure to use `/fs1/projects/{project-name}/` which lives on the parallel file-system storage, and ensure your scripts and notebooks use the correct paths to manage your data.
```

## Running Jupyter Notebook

To begin, SSH to the login node. Instructions on how to connect are provided in the welcome Email.

As the compute nodes where workloads run on the cluster are not directly reachable from the campus network, we need to set up a chain of SSH port forwards to access Jupyter Notebook instances. The job script will:
1. Start a Jupyter notebook server on an available port on the compute node
2. Provide you with the necessary SSH command to establish connection through:
  - The Linux lab machine (adams)
  - The login node
  - Finally reaching your compute node

Save the following script as `jupyter.sbatch`:

```bash
#!/bin/bash

#SBATCH --nodelist=<compute-node>
#SBATCH --gpus=2
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --time=00:30:00
#SBATCH --job-name=jupyter_notebook
#SBATCH --output=/home/%u/%x_%j.out
#SBATCH --error=/home/%u/%x_%j.err

# Connection variables
LOGIN_NODE="<login-node-address>"  # Replace between the " " with the login node's address from the welcome Email
LOGIN_PORT="<login-port>"          # Replace between the " " with port number from welcome Email
XX="<xx>"                          # Replace between the " " with a number from 01-30

module load jupyter

# Check for a port's availability
check_port() {
   local port=$1
   nc -z localhost $port
   if [ $? -eq 0 ]; then
       return 1
   else
       return 0
   fi
}

# Find an available port
port=8888
while ! check_port $port; do
   port=$((port + 1))
done

# Get node information
compute_node=$(hostname -f)
user=$(whoami)

# Print connection instructions
echo "==================================================================="
echo "To connect to your Jupyter notebook, run this command on your local machine:"
echo ""
echo "ssh -N -L ${port}:${compute_node}:${port} -J ${user}@adams204${XX}.hofstra.edu:${LOGIN_PORT},${user}@${LOGIN_NODE}:${LOGIN_PORT} ${user}@${LOGIN_NODE}"
echo ""
echo "When finished, clean up by running this command on the login node:"
echo "scancel ${SLURM_JOB_ID}"
echo "==================================================================="

# Start Jupyter notebook
jupyter notebook --no-browser --port=${port} --ip=0.0.0.0
```

```warning
### Don't forget to replace the placeholders!

The <...> placeholders need to be replaced with what _you_ need. For instance:
- <login-node-address> needs to be replaced with the address of the login node provided in your welcome Email
- <login-port> needs to be replaced with the port number from your welcome Email
- <xx> needs to be replaced with a number between 01-30 (inclusive)
- <compute-node> needs to be replaced with an available compute node from the cluster nodes list. You can find the full list of nodes on the [About Star page]({{site.baseurl}}{% link quickstart/about-star.md %}).
- Make sure you change the path for both the `--output` and `--error` directives to where _you_ would like the files to be saved at.
```

The script uses these Slurm settings:
- `--nodelist`: Specifies which compute node to use (e.g., `gpu1` or `cn01`)
- `--gpus=2`: This enables us to use 2 of the GPUs on the specified node. See each node's GPU information [here]({{site.baseurl}}{% link quickstart/about-star.md %}). Without this specification, you cannot see or use the GPUs on the compute node. Feel free to replace this number with another **valid option**.
- `--ntasks=1`: Runs one instance of Jupyter
- `--cpus-per-task=1`: Uses one CPU thread (Hyperthreading is enabled on the compute nodes)
- `--time=00:30:00`: Sets a 30-minute time limit for the job (The format is `hh:mm:ss`)

To use the script:

1. Replace all placeholders in the script as indicated in the warning above.

2. Submit the job:
```bash
sbatch jupyter.sbatch
```
Upon your job's submission to the queue, you will see the output indicating your job's ID. You need to replace _your_ job ID value with the `<jobid>` placeholder throughout this documentation.

3. Check your output file (`jupyter_notebook_<jobid>.out`) for the SSH command:
```bash
cat jupyter_notebook_<jobid>.out  # Run this command in the directory the .out file is located.
```

4. Open a new terminal on your local machine and run the SSH command provided in the output file. If prompted for a password, use your Linux lab password if you haven't set up SSH keys. Note that the command will appear to hang after successful connection - this is the expected behavior. Do not terminate the command (Ctrl + C) as this will disconnect your Jupyter notebook session (unless you intend to do so).

5. Check the error file on the login node for the Jupyter URL:
```bash
cat jupyter_notebook_<jobid>.err  | grep '127.0.0.1' # Run this command in the directory the .err file is located.
```
```warning
### Wait a moment!
Make sure you wait about 30 seconds after executing the SSH portforwarding command on your local machine. It takes the `.err` file a little time to be updated and include your link.
```

6. Copy the URL from the error file and paste it into your **local machine's browser**.

7. If you're done prior to the job's termination due to the walltime, clean up your session by running this command on the login node:
```bash
scancel <jobid>
```
Afterwards, press `Ctrl + C` on your local computer's terminal session that you set up port forwarding.

## Using Existing Container Images

You can also run Docker images on the cluster through [Apptainer](https://apptainer.org/) (a variant of Singularity). This is great when you want an environment with everything pre-installed. Check out [the Apptainer Guide]({{site.baseurl}}{% link software/apptainer.md %}) to learn more.
