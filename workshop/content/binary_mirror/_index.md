---
title: "Create a Binary Mirror"
date: 2020-07-30T10:46:30-04:00
weight: 600
pre: "<b>III ⁃ </b>"
tags: ["HPC", "Application", "Spack", "FSx", "Lustre"]
---

## Create a Binary Mirror with FSx Lustre and Amazon S3

<!-- Diagram (FSx -> S3 -> Spack) -->
![Spack Binary Mirror](/images/binary_mirror/binary_cache.svg)

In our default installation, Spack installs everything from source. For large packages or packages with lots of dependencies, this can take forever. To speed this up, Spack can cache pre-built binaries on Amazon S3. In this guide we'll create a [spack mirror](), build binaries for **openfoam** and export them to the mirror. We'll be using Amazon S3 for the mirror and FSx Lustre as our filesystem. FSx Lustre has [data repository](https://docs.aws.amazon.com/fsx/latest/LustreGuide/fsx-data-repositories.html) functionality that can automatically back up the contents of the filesystem to S3. We'll use this functionality to build our binaries locally on the cluster, then export them to S3 for future consumption.

## Steps:
1. Create an S3 bucket to serve as the mirror
2. Create a cluster with AWS ParallelCluster
3. Attach FSx Lustre
4. Built the binaries, using the compute nodes to speedup the process
5. Export those binaries to Amazon S3
