+++
title = "d. Configure Cloud9"
date = 2019-09-18T10:46:30-04:00
weight = 40
tags = ["tutorial", "spack"]
+++

## 1. Cloud9
In the previous section we configured a Cloud9 machine. This is a Ubuntu 18.04 based-virtual machine running in AWS Elastic Compute Cloud (EC2).

Cloud9 is a powerful Integrated Development Environment (IDE) where you can write, run, and debug code via your browser. Cloud9 provides the software and tooling needed for dynamic programming languages including JavaScript, Python, PHP, Ruby, Go, and C++. It sets up AWS credentials and automatically stops during periods of inactivity (defaults to 30 mins). Visit the [AWS Cloud9 Features page](https://aws.amazon.com/cloud9/details/) for more information.

![Cloud9](/images/cloud9.png)

Now we're going to setup the instance for this tutorial:

{{% notice info %}}
**Pro Tip:** To change the theme click on the <i class="fas fa-cog"></i>  icon in the upper right and click "Themes" on the prefences pane, and select your favorite, my personal favorite is "Jett":
![Cloud9](/images/theme.png)
{{% /notice %}}

## 2. Grow the Root Partition 

By default the root volume store (`/`) of an EC2 instance is 10 GB. We're going to increase that to 30GB in order to install packages with spack.

```bash
wget https://spack-tutorial.workshop.aws/scripts/grow.sh
bash grow.sh # increase to 30 GB

# ...
df -h
```
