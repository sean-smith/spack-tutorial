+++
title = "c. AWS ParallelCluster"
date = 2019-09-18T10:46:30-04:00
weight = 40
tags = ["tutorial", "spack"]
+++

## The Cluster

So what did we just create?

Well, roughly this:

![pcluster](/images/spack_on_pcluster/aws_pcluster.svg)

Specifically you'll notice on the Cloud9 instance there's a file `~/environment/config.ini`, when we open it we see:

```bash
vim ~/environment/config.ini
```

![pcluster config](/images/spack_on_pcluster/config.png)

This is the cluster's configuration file. It defines everything about the cluster, such as:

####  The `hpc-cluster` HPC Environment

- [Ubuntu 18.04](https://ubuntu.com/aws) operating system (`apt`),
- [SLURM](https://slurm.schedmd.com/documentation.html) Workload Manager (`sbatch`, `srun`, `squeue`, `sinfo`),
- [Environment Modules](http://modules.sourceforge.net/) (`module avail`),
- [Spack](https://spack.io/) Package Manager (`spack`),
- [FSx Lustre](https://aws.amazon.com/fsx/lustre/) scratch filesystem (`/scratch`),
- A Master node for job scheduling, interactive development, and
- Multiple Compute nodes that scale up from 0 based on the jobs you submit. 

### Master and Compute Nodes

The default `hpc-cluster` configuration is described in the table below:

| Node | # Nodes | Longevity | Instance Type | EFA |  <ul>Purpose</ul>  |
|---|---|---|---|---|---| 
|  Master  |  1 | persistent |  c5.2xlarge  |  No  | <ul><li>Job Submission</li></ul><ul><li>Interactive Analysis and Development</li></ul><ul><li>Visualization</li></ul> |
| Compute |  0 - 10 | auto-scaling |  c5.18xlarge | No |  <ul><li>Batch Processing</li></ul><ul><li>Tightly or Loosely Coupled Job Processing</li></ul>


### Filesystem Shares
The `hpc-cluster` Master and Compute nodes share a number of common filesystems as described in the table below. Leverage shared filesystems to reduce data movement and duplication. 

| Path  |  Recommended Use |  Storage Backend |  Size  |
|---|---|---|---|
| /home  |   |  EBS (gp2) | 200 GB |  
| /shared | Application source code, compiled binaries, user-space software and job scripts  |  EBS (gp2) | 500 GB  |  
| /scratch | Reference data and intermediate output from jobs  | FSx Lustre (SCRATCH2) | 1.2 TB |   |
| /opt/slurm | Do NOT write to this path  |  - |  - |  
| /opt/intel | Do NOT write to this path  |  - | - |  
| s3://aws-hpc-quickstart-datarepository****** | Input files and final output files. <br>💡Note: this bucket is not directly mounted, but instances do have read/write access via the `aws s3` command  | S3 | N/A |  

To identify mounted filesystems and the available storage on the cluster:

```bash
showmount -e localhost
df -h
```

![Filesystems](/images/file_systems.png)
