+++
title = "d. Spack Intro"
date = 2019-09-18T10:46:30-04:00
weight = 40
tags = ["tutorial", "spack"]
+++

## 1. Check Spack Installation

Spack is already installed on the cluster, to check the version run:

```bash
$ spack --version
0.15.0
```

## 2. Install Software

On the cluster, the `ubuntu` user has full permissions to install software via the sudo command. Note, however, in order to use software in a batch scheduled job, you must either:

1. Install the software on a shared filesystem common to Master and Compute nodes, or
2. Install the software individually on every compute node.

We recommend 1. To this end, we provide `/shared` as a shared file system common to Master and Compute nodes. Furthermore, we have pre-configured Spack to install packages to `/shared/spack` and make them accessible available as a Modulefile.

For example, to install SRA-Toolkit, you might use the following workflow to search for, and then install the package:

```bash
spack list sra
spack install sra-toolkit
```

![Spack install SRA](https://user-images.githubusercontent.com/187202/79090384-1ba4fb80-7d0f-11ea-9e79-a5251ee2d454.png)

Once it's been installed you can load and unload SRA-Toolkit with 

```bash
module avail
module load intelmpi sra-toolkit/2.9.6-gcc-7.5.0
```

![](https://user-images.githubusercontent.com/187202/79100229-a09e0e00-7d2b-11ea-9775-3b0ab8a4e522.png)