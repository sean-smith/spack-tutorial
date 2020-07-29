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
0.15.3
```
## 2. Use Binary Mirror

By default Spack installs everything from source, to install things faster by using a binary mirror, run:

```bash
export AWS_REGION=$(curl -s http://169.254.169.254/latest/dynamic/instance-identity/document | grep -oP '\"region\"[[:space:]]*:[[:space:]]*\"\K[^\"]+')
. /shared/spack-1share/spack/setup-tutorial-env.sh # this errors, just hit Ctrl-c
spack mirror add tutorial s3://spack-tutorial-container/mirror/
spack gpg trust /shared/spack-0.15/share/spack/keys/tutorial.pub
```

## 3. Install Software

On the cluster, the `ubuntu` user has full permissions to install software via the sudo command. Note, however, in order to use software in a batch scheduled job, you must either:

1. Install the software on a shared filesystem common to Master and Compute nodes, or
2. Install the software individually on every compute node.

We recommend option 1, to this end, we've mounted `/shared` as a shared file system common to Master and Compute nodes. Furthermore, we have pre-configured Spack to install packages to `/shared/spack` and make them accessible available as a Modulefile.

For example, to install `SRA-Toolkit`, you might use the following workflow to search for, and then install the package:

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