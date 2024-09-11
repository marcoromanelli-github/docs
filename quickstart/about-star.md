---
sort: 1
---

# About Star

## Overview

The Star HPC Cluster is a computing facility designed for a variety of research and computational tasks. It combines advanced computing **nodes** and a high-speed **storage system** with a suite of **software applications**.

SLURM (Simple Linux Utility for Resource Management) is our chosen job scheduler and queueing system that efficiently manages resource allocation, ensuring everyone gets the right amount of resources at the right time.

Apptainer (formerly Singularity) is also a major application on the cluster. Apptainer is a containerization platform similar to Docker with the major difference that it runs under user privileges instead of `root`. This platform is enhanced by NGC (NVIDIA GPU Cloud) which provides access to a wide array of pre-built, GPU-optimized software containers for diverse applications. This integration saves all users a lot of time as they don’t need to set up the software applications from scratch and can just pull and use the NGC images with Apptainer.

The cluster also supports various software applications tailored to different needs: Python and R for data analysis, MATLAB for technical computing, Jupyter for interactive projects, and OpenMPI for parallel computing. Anaconda broadens these capabilities with packages for scientific computing, while NetCDF manages large datasets. For big data tasks, Hadoop/Spark offers powerful processing tools.

## Hardware

### Login Node

### Compute Nodes

- Two Apollo 6500 Gen10+ HPE nodes, _each_ containing 8 NVIDIA A100 SXM GPUs.
- One HPE ProLiant DL385 Gen10+ v2, containing 2 NVIDIA A30 SXM GPUs.
- One HPE A30 node, containing 2 NVIDIA A30 GPUs.
- Two HPE Apollo 6500 nodes, _each_ containing 8 NVIDIA A100 GPUs.
- Two DL385s nodes, _each_ containing 2 NVIDIA PCIe H100 GPUs.
- Two Cray XD665 nodes, _each_ containing 4 NVIDIA HGX H100 80GB GPUs.
- One Cray XD670 node, containing 8 NVIDIA HGX H100 80GB GPUs.

#### HPE Apollo 6500 Gen10

| Attribute\Node Name           | gpu1                                                           | gpu2                                                           |
| ----------------------------- | -------------------------------------------------------------- | -------------------------------------------------------------- |
| Model Name                    | HPE ProLiant XL675d Gen10 Plus; Apollo 6500 Gen10 Plus Chassis | HPE ProLiant XL675d Gen10 Plus; Apollo 6500 Gen10 Plus Chassis |
| Sockets                       | 2                                                              | 2                                                              |
| Cores per Socket              | 32                                                             | 32                                                             |
| Threads per Core              | 2                                                              | 2                                                              |
| Memory                        | 1024 GiB Total Memory (16 x 64GiB DIMM DDR4)                   | 1024 GiB Total Memory (16 x 64GiB DIMM DDR4)                   |
| GPU                           | 8 SXM NVIDIA A100s                                             | 8 SXM NVIDIA A100s                                             |
| Local Storage (Scratch space) | 407GB                                                          | 407GB                                                          |

#### HPE DL385 Gen10

| Attribute\Node Name           | cn01                                       |
| ----------------------------- | ------------------------------------------ |
| Model Name                    | HPE ProLiant DL385 Gen10 Plus v2           |
| Sockets                       | 2                                          |
| Cores per Socket              | 32                                         |
| Threads per Core              | 2                                          |
| Memory                        | 256GiB Total Memory (16 x 16GiB DIMM DDR4) |
| GPU                           | 2 SXM NVIDIA A30s                          |
| Local Storage (Scratch Space) | 854G                                       |

#### HPE A30

| Attribute\Node Name | a30node           |
| ------------------- | ----------------- |
| Model Name          | HPE A30 Node      |
| GPUs                | 2 NVIDIA A30 GPUs |

#### HPE Apollo 6500

| Attribute\Node Name | apollo6500node1      | apollo6500node2      |
| ------------------- | -------------------- | -------------------- |
| Model Name          | HPE Apollo 6500 Node | HPE Apollo 6500 Node |
| GPUs                | 8 NVIDIA A100 GPUs   | 8 NVIDIA A100 GPUs   |

#### DL385s

| Attribute\Node Name | dl385node1              | dl385node2              |
| ------------------- | ----------------------- | ----------------------- |
| Model Name          | DL385s Node             | DL385s Node             |
| GPUs                | 2 NVIDIA PCIe H100 GPUs | 2 NVIDIA PCIe H100 GPUs |

#### Cray XD665

| Attribute\Node Name | xd665node1                  | xd665node2                  |
| ------------------- | --------------------------- | --------------------------- |
| Model Name          | Cray XD665 Node             | Cray XD665 Node             |
| GPUs                | 4 NVIDIA HGX H100 80GB GPUs | 4 NVIDIA HGX H100 80GB GPUs |

#### Cray XD670

| Attribute\Node Name | xd670node                   |
| ------------------- | --------------------------- |
| Model Name          | Cray XD670 Node             |
| GPUs                | 8 NVIDIA HGX H100 80GB GPUs |

### Storage System

Our storage system contains of four HPE PFSS nodes, collectively offering a total of 63TB of storage. You can think of these four nodes as one unified 63TB storage unit as it is a **Parallel File System Storage** component. These nodes work in parallel and are all mounted under **one** mount point on the gpu nodes only (`/fs1`).

## Our vision

Making complex and time-intensive calculations simple and accessible.

### Our Goal

Our heart is set on creating a community where our cluster is a symbol of collaboration and discovery. We are wishing to provide a supportive space where researchers and students can express their scientific ideas and explore unchanted areas. We aim to make the complicated world of computational research a shared path of growth, learning, and significant discoveries for the ones that are eager to learn.

## Operations Team

- Alexander Rosenberg
- Mani Tofigh

## The Board

- Edward H. Currie
- Daniel P. Miller
- Adam C. Durst
- Jason D. Williams
- Thomas G. Re
- Oren Segal
- John Ortega
