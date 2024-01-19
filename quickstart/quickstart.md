---
sort: 2
---

# Quick Start Guide

## Account Access

A Star HPC account is required to access and submit jobs to the Star HPC cluster.  If you do not have one or need help with your account, please contact your cluster administrator.

The application process may require justification of the need for HPC resources, detailing the kind of work you intend to do, the resources you expect to use, and sometimes, the anticipated outcomes of your research.

Access to the cluster comes with responsibilities and certain privileges. Your account comes the responsibility to use the resources wisely and efficiently, to respect the shared nature of the environment, and to contribute to the overall HPC community.

Users should understand the policies on data privacy and user responsibilities.

## Getting started on the machine (account, quota, password)

Before you start using the Star cluster, please read Hofstra University's [Acceptable Use Guidelines](http://www.hofstra.edu/scs/aug).

A general introduction to the Star HPC cluster, research community, and support group be found at [starhpc.hofstra.io](https://starhpc.hofstra.io).

## How to get an account and a CPU quota on Star

To be able to work on the Star cluster, you must have an account and you must have been granted CPU time on the system. There are two ways to achieve this:

### Hofstra Quota

“Local users” (i.e. users from Hofstra or affiliated with Hofstra in some
way) can apply for an account and a quota from Hofstra’s share of Star. If
you want to apply for such a quota follow the instructions here:

* [How to get a local account on Star](huquota.html)

### Research Council Quota

Regional users (including users from Hofstra) may apply for an account and a
CPU quota from the Research Council's share of Star. If you want to
apply for such a quota please use this [regional quota form](https://www.example.com/application/project/).

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

