+++
title = "a. Workshop Initial Setup"
date = 2019-09-18T10:46:30-04:00
weight = 20
tags = ["tutorial", "spack"]
+++

### Step 1
To get started click "Create Stack":

| Region       | Stack                                                                                                                                                                                                                                                                                                              |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| us-east-1    | [![amplifybutton](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=AWS-HPC-Quickstart&templateURL=https://notearshpc-quickstart.s3.amazonaws.com/cfn.yaml&param_ConfigS3URI=s3://seaam/config.ini)       |

### Step 2
If you have not authenticated with the AWS Management Console, you will be prompted to login with the AWS Account ID or alias, IAM user name, and password that was provided to you.

### Step 3
Scroll to the bottom of the page, and leave most of the Parameters as they are.

1. Enter an initial `BudgetLimit` for the project. This will be used to track spending and send an alert when you cross 80%.
2. Update `NotificationEmail` that will receive budget notifications.
3. Fill out the `UserPasswordParameter` with a temporary password. Keep it simple! You will be prompted to change it on first use.
4. Select `I acknowledge that AWS Cloudformation might create IAM resources`.
5. Now, click `Create Stack` to deploy the QuickStart Environment.

![CloudFormation Create Stack](/images/create_stack.png)

### Deployment
Deployment takes about 15 minutes. This QuickStart provisions:

* A Cloud9 Integrated Development Environment (IDE) in the selected region;
* an AWS Parallel Cluster environment, named `hpc-cluster`, pre-configured with Spack;

Go get a cup of ☕️ while waiting for the stacks to complete.

Provisioning is complete when all Stacks show `CREATE_COMPLETE`.

![CloudFormation CREATE_COMPLETE](/images/cfn_console.png)

### Access Cluster

When all stacks show `CREATE_COMPLETE`, click on the Outputs tab of the `AWS-HPC-Quickstart` stack. 

Click on the `ResearchWorkspaceURL` to go directly to the Cloud9 Instance that was created. 

![CloudFormation Outputs Tab](/images/cfn_output.png)

<!-- 
This stack creates a tightly-coupled cluster with the following config:

```ini
[global]
cluster_template = hpc
update_check = true
sanity_check = true

[aws]
aws_region_name = ${AWS_DEFAULT_REGION}

[aliases]
ssh = ssh {CFN_USER}@{MASTER_IP} {ARGS}

[cluster hpc]
key_name = ${ssh_key_id}
base_os = ubuntu1804
scheduler = slurm
master_instance_type = c5.2xlarge
compute_instance_type = c5n.18xlarge
vpc_settings = public-private
fsx_settings = fsx-scratch2
disable_hyperthreading = true
dcv_settings = dcv
post_install = ${post_install_script_url}
post_install_args = "/shared/spack-0.13 /opt/slurm/log sacct.log"
s3_read_resource = arn:aws:s3:::*
s3_read_write_resource = ${s3_read_write_resource}/*
initial_queue_size = 0
max_queue_size = 10
placement_group = DYNAMIC
master_root_volume_size = 200
compute_root_volume_size = 80
ebs_settings = myebs
cw_log_settings = cw-logs
enable_efa = compute

[ebs myebs]
volume_size = 500
shared_dir = /shared

[dcv mydcv]
enable = master

[fsx fsx-scratch2]
shared_dir = /scratch
storage_capacity = 1200
deployment_type = SCRATCH_2
import_path=${s3_read_write_url}

[dcv dcv]
enable = master
port = 8443
access_from = 0.0.0.0/0

[cw_log cw-logs]
enable = false

[vpc public-private]
vpc_id = ${vpc_id}
master_subnet_id = ${master_subnet_id}
compute_subnet_id = ${compute_subnet_id}
``` -->