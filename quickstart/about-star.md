# About Star

## Introduction

Star HPC Cluster is a computing facility designed to cater to a wide range of computational and data-intensive research needs. It serves as a powerful tool for scientists, engineers, and researchers, enabling them to tackle complex scientific problems that require substantial computational resources. The cluster combines computing `nodes` and a high-speed `storage system` with a suite of versatile software applications.

At Star we need an effective management and utilization of resources, and we facilita SLURM for this purpose. SLURM (Simple Linux Utility for Resource Management) is a job scheduler that manages the allocation of computational resources. It ensures that users' computational tasks are queued and processed effectively, and everyone gets their fair share of resource at the right time.

Apptainer (formerly known as Singularity) is also used intensively in the cluster. Singularity/Apptainer provides a containerization platform that allows users to create and deploy applications in a consistent, reproducible, and portable manner. If you have used Docker before, Singularity is not going to be a new concept. Singularity is pretty much Docker, except it runs the containers under users’ system privileges, unlike Docker that runs everything under `root`. Furthermore, in conjunction with NGC (NVIDIA GPU Cloud), Singularity/Apptainer enables users to leverage a wide array of pre-built containers. NGC offers a comprehensive catalog of GPU-optimized software containers for deep learning, machine learning, and HPC applications. This integration means that users have access to dozens of ready-to-use containers, eliminating the need to set up these applications from scratch.

The cluster is also equipped with a range of software applications, each serving specific purposes. For instance, Python and R are included for data analysis and statistical computing, MATLAB for high-level technical computing, Jupyter for interactive data science and scientific computing, and OpenMPI for efficient parallel computing. Anaconda offers a comprehensive package for scientific computing and data science in Python and R, while NetCDF is vital for the manipulation and storage of large scientific datasets. For handling big data, Hadoop/Spark is also available.

## Hardware

### Login Node

### Compute Nodes

#### HPE Apollo 6500 Gen10

#### HPE DL365 Gen10

### Storage System

## Software

### SLURM

### Singularity/Apptainer

### Python

### Jupyter

### MATLAB

### R

### OpenMPI

### Anaconda

### NetCDF

### Hadoop/Spark
