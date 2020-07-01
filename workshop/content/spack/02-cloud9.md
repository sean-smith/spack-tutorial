+++
title = "a. Cloud9"
date = 2019-09-18T10:46:30-04:00
weight = 30
tags = ["tutorial", "spack"]
+++

## 1. Cloud9
In the previous section we created a hpc cluster using AWS CloudFormation and AWS ParallelCluster.

Navigate to your `ResearchWorkspaceUrl`, as shown at the end of the last section, and your web browser will be re-directed to an AWS Cloud9 interactive console like this:

<!-- ![Cloud9](https://user-images.githubusercontent.com/187202/79025821-81955580-7b4c-11ea-9f2f-3fd128afe939.png) -->

Cloud9 is a powerful Integrated Development Environment where you can write, run, and debug code via your browser. Cloud9 provides the software and tooling needed for dynamic programming languages including JavaScript, Python, PHP, Ruby, Go, and C++. Visit the [AWS Cloud9 Features page](https://aws.amazon.com/cloud9/details/) for more informatiom. 

## 2. Connect to the HPC Environment

Confirm the name of your AWS Parallel Cluster: 
```bash
pcluster list
```
The name should be `hpc-cluster` and the state should be `CREATE_COMPLETE`. 

<!-- ![pcluster list](https://user-images.githubusercontent.com/187202/79089316-59078a00-7d0b-11ea-9695-c39c50b95b6f.png) -->

Connect to the cluster: 
```bash
pcluster ssh hpc-cluster
```

<!-- ![pcluster ssh](https://user-images.githubusercontent.com/187202/79089604-55283780-7d0c-11ea-9709-e5a6a091ff73.png) -->

{{% notice info %}}
The Cloud9 Instance is not the Master Instance of the cluster, to connect to the master instance you'll need to run `pcluster ssh hpc-cluster`.
{{% /notice %}}

