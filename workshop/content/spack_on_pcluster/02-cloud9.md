+++
title = "b. Cloud9"
date = 2019-09-18T10:46:30-04:00
weight = 30
tags = ["tutorial", "spack"]
+++

## 1. Cloud9
In the previous section we created a hpc cluster using AWS CloudFormation and AWS ParallelCluster.

Navigate to your `ResearchWorkspaceUrl`, as shown at the end of the last section, and your web browser will be re-directed to an AWS Cloud9 interactive console like this:

You'll see a popup about **AWS Managed Temporary Credentials**. Click **Enable**

![Cloud9](/images/spack_on_pcluster/cloud9_console.png)

Cloud9 is a powerful Integrated Development Environment where you can write, run, and debug code via your browser. Cloud9 provides the software and tooling needed for dynamic programming languages including JavaScript, Python, PHP, Ruby, Go, and C++. Visit the [AWS Cloud9 Features page](https://aws.amazon.com/cloud9/details/) for more information.


{{%expand "💡 Pro Tip: Change Cloud9 Theme" %}}
 To get this awesome black theme click on the <i class="fas fa-cog"></i>  icon in the upper right and click "Themes" on the prefences pane, and select "Jett":
![Cloud9](/images/theme.png)
{{% /expand%}}

## 2. Connect to the HPC Environment

Confirm the name of your AWS Parallel Cluster: 
```bash
pcluster list --color
```
The name should be `hpc-cluster` and the state should be `CREATE_COMPLETE`. It may be `hpc-cluster-*` if it's the second cluster created on the account.

![pcluster list](/images/pcluster_list.png)

Connect to the cluster: 
```bash
pcluster ssh hpc-cluster
```

![pcluster ssh](/images/pcluster_ssh.png)

{{% notice info %}}
The Cloud9 Instance is not the Master Instance of the cluster, to connect to the master instance you'll need to run `pcluster ssh hpc-cluster`.
{{% /notice %}}

