+++
title = "a. Create a Cluster"
date = 2019-09-18T10:46:30-04:00
weight = 20
tags = ["tutorial", "spack", "fsx", "parallelcluster"]
+++

[AWS FSx for Lustre](https://aws.amazon.com/fsx/lustre/) is a managed Lustre filesystem backed by S3. We can automatically create a filesystem and have it mounted to our cluster by modifying the parallelcluster config file. 

## Step 1

Now we're going create an AWS ParallelCluster config file that uses the previously created bucket. First edit the file `config.ini` and change the lines:

```bash
$ vim config.ini

[cluster hpc]
...
# change the first argument to /spack
post_install_args = "/spack releases/v0.15 /opt/slurm/log sacct.log" 
# change to arn:aws:s3:::spack-tutorial-mirror/*
s3_read_write_resource = arn:aws:s3:::spack-tutorial-mirror/*

[fsx fsx-scratch2]
shared_dir = /spack   # change this to /spack
...
import_path=s3://spack-tutorial-mirror   # change this line
export_path = s3://spack-tutorial-mirror # add this line
```

1. We change the **post_install_args** to install spack with a root directory of `/spack`
2. In the fsx section we change **shared_dir** from  `/shared` to `/spack`
3. In the fsx section we change the **export_path** and **import_path** to point to the newly created bucket
4. We add read/write permissions to the bucket so we can see what files get added `s3_read_write_resource = arn:aws:s3:::spack-tutorial-mirror/*`

## Step 2

Create the cluster:

```bash
pcluster create spack-binary-mirror
```

When the cluster is finised creating you'll see:

![create complete](/images/binary_mirror/create_complete.png)