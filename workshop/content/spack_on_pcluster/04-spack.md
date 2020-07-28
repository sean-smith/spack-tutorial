+++
title = "e. Spack Intro"
date = 2019-09-18T10:46:30-04:00
weight = 40
tags = ["tutorial", "spack"]
+++

## 1. Check Spack Installation

Spack is already installed on the cluster, to check the version run:

```bash
$ spack --version
0.15.2
```

## 2. Install Software

On the cluster, the `ubuntu` user has full permissions to install software via the sudo command. Note, however, in order to use software in a batch scheduled job, you must either:

1. Install the software on a shared filesystem common to Master and Compute nodes, or
2. Install the software individually on every compute node.

We recommend 1. To this end, we've mounted `/shared` as a shared file system common to Master and Compute nodes. Furthermore, we have pre-configured Spack to install packages to `/shared/spack` and make them accessible available as a Modulefile.

For example, to install SRA-Toolkit, you might use the following workflow to search for, and then install the package:

```bash
spack list sra
spack install sra-toolkit
```

![Spack install SRA](/images/spack_on_pcluster/spack_sra.png)

Once it's been installed you can load and unload SRA-Toolkit with 

```bash
module avail
module load intelmpi sra-toolkit/2.9.6-gcc-7.5.0
```

![Module Load Intel MPI and SRA](/images/spack_on_pcluster/sra_load.png)