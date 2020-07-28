---
title: "Spack on AWS ParallelCluster"
date: 2019-01-24T09:05:54Z
weight: 500
pre: "<b>II ⁃ </b>"
tags: ["HPC", "Application", "Spack"]
---

![AWS ParallelCluster](/images/spack_on_pcluster/pcluster.png)

[AWS ParallelCluster](https://aws.amazon.com/hpc/parallelcluster/) is an AWS supported Open Source cluster management tool that makes it easy for you to deploy and manage High Performance Computing (HPC) clusters in the AWS cloud. Built on the Open Source CfnCluster project, AWS ParallelCluster enables you to quickly build an HPC compute environment in AWS. It automatically sets up the required compute resources, shared filesystem and scheduler.

In the following guide, we:

1. Setup a cluster with AWS ParallelCluster
2. Log in to that cluster via Cloud9
3. Use Spack to install some packages
4. Delete all created resources
