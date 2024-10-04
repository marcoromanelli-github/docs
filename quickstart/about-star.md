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

- IBM System x3550 with 128GB RAM
- DL325 Gen10+ with an 8-core EPYC processor, 128GB RAM
- DL385 Gen10+ v2 with 2x AMD 32-core EPYC processors, 256GB RAM

### Compute Nodes

- 1x HPE DL385 Gen10+ v2 node with 2x NVIDIA A30 24GB PCIe GPUs, 2x AMD EPYC 32-core processors, 256GiB DDR4 RAM
- 2x HPE XL675d Gen10+ nodes (Apollo 6500 Gen10+ chassis), each with 8x NVIDIA A100 80GB SXM GPUs, 1TiB 3200 DDR4 RAM, 2x 480GB SSD
- 2x HPE DL380a Gen11, each with 2x NVIDIA H100 80GB PCIe GPUs, 2x Intel Xeon 32-core processors, 512GiB DDR5 RAM
- 2x Cray XD665 nodes, each with 4x NVIDIA H100 80GB SXM GPUs, 2x AMD EPYC 32-core processors, 768GiB DDR5 RAM
- 1x Cray XD670 node with 8x NVIDIA H100 80GB SXM GPUs, 2x Intel Xeon 32-core processors, 2TiB DDR5 RAM

#### HPE Apollo 6500 Gen10

| Attribute\Node Name           | gpu1 and gpu2                                                           |
| ----------------------------- | -------------------------------------------------------------- |
| Model Name                    | HPE ProLiant XL675d Gen10 Plus; Apollo 6500 Gen10 Plus Chassis |
| Processors                 | AMD EPYC 7513                                               |
| Sockets                       | 2                                                              |
| Cores per Socket              | 32                                                             |
| Threads per Core              | 2                                                              |
| Memory                        | 1024 GiB Total Memory (16 x 64GiB DIMM DDR4)                   |
| GPU                           | 8 SXM NVIDIA A100s                                             |
| Local Storage (Scratch space) | 6.4TB (5.8TiB) SSD                                                          |

#### HPE DL385 Gen10

| Attribute\Node Name           | cn01                                       |
| ----------------------------- | ------------------------------------------ |
| Model Name                    | HPE ProLiant DL385 Gen10 Plus v2           |
| Processors                  | AMD EPYC 7513 32-Core Processor            |
| Sockets                       | 2                                          |
| Cores per Socket              | 32                                         |
| Threads per Core              | 2                                          |
| Memory                        | 256GiB Total Memory (16 x 16GiB DIMM DDR4) |
| GPU                           | 2 SXM NVIDIA A30s                          |
| Local Scratch | None                                       |

#### HPE DL380a Gen11

| Attribute\Node Name           | gpu3 and gpu4                             |
| ----------------------------- | -------------------------------------------- |
| Model Name                    | HPE DL380a Gen11                             |
| Processors              | 64 Physical cores / 128 Logical Cores (2 x Intel Xeon-P 8462Y+ @ 2.8GHz)  |
| Memory                        | 512 GiB DDR5 RAM                             |
| GPU                           | 2 NVIDIA H100 80GB GPUs (NVAIE subscription) |
| Network                       | 4-port GbE, 1-port HDR200 InfiniBand         |
| Local Storage (Scratch Space) | None                                    |

#### Cray XD665 Nodes

| Attribute\Node Name           | gpu5 and gpu6                      |
| ----------------------------- | -------------------------------------- |
| Model Name                    | Cray XD665                             |
| Processors  | 64 Physical cores / 128 Logical Cores (2 x AMD EPYC Genoa 9334 @ 2.7GHz)                  |
| Memory                        | 768 GiB DDR5 RAM                       |
| GPU                           | 4 NVIDIA HGX H100 80GB SXM GPUs        |
| Network                       | 2-port 10GbE, 1-port HDR200 InfiniBand |
| Local Storage (Scratch Space) | None                          |

#### Cray XD670 Node

| Attribute\Node Name           | gpu7                                |
| ----------------------------- | -------------------------------------- |
| Model Name                    | Cray XD670                             |
| Processors  | 64 Physical cores / 128 Logical Cores (2 x Intel Xeon-P 8462Y+ @ 2.8GHz)                  |
| Memory                        | 2048 GiB DDR5 RAM                      |
| GPU                           | 8 NVIDIA HGX H100 80GB SXM GPUs        |
| Network                       | 2-port 10GbE, 1-port HDR200 InfiniBand |
| Local Storage (Scratch Space) | None                                |

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
