# Quick Start Guide

## Account Access

A Star HPC account is required to access and submit jobs to the Star HPC cluster.  If you do not have one or need help with your account, please contact your cluster administrator.

The application process may require justification of the need for HPC resources, detailing the kind of work you intend to do, the resources you expect to use, and sometimes, the anticipated outcomes of your research.

Access to the cluster comes with responsibilities and certain privileges. Your account comes the responsibility to use the resources wisely and efficiently, to respect the shared nature of the environment, and to contribute to the overall HPC community.

Users should understand the policies on data privacy and user responsibilities.


## Login node

Access to the cluster is provided through SSH to the login node. The login node serves as the gateway or entry point to the cluster. It is important to understand that the login node is not for running computationally intensive tasks itself. Instead, it is for tasks such as file management, editing, and job submission. The actual computational work is done on the compute nodes, which you access indirectly by submitting jobs through Slurm, the job scheduling system.

## Scheduler policies

Users should understand the cluster's policies regarding CPU and GPU usage, including time limits and priority settings.

Users are advised to learn how to check their usage statistics to manage the resource allocation efficiently.

## Storage policies

Storage quotas and usage limits are put in place to ensure fair use and equitable distribution of the resources among all users.

It is important to know where to store different types of data (such as large datasets or temporary files).

Your home directory provides a limited amount of storage for scripts, source code, and small datasets.

Project-specific directories may be created upon request for shared storage among multiple accounts.

## Further Reading

To make proper use of the cluster, please familiarize yourself with the basics of using Slurm, fundamental HPC concepts, and the cluster's architecture.

You may be familiar with the `.bashrc`, `.bash_profile`, or `.cshrc` files for environment customization. To support different environments needed for different software packages, environment modules are used. Modules allow you to load and unload various software environments tailored to your computational tasks.

