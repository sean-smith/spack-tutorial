+++
title = "a. Workshop Initial Setup"
date = 2019-09-18T10:46:30-04:00
weight = 10
tags = ["tutorial", "spack"]
+++

### Step 1
To get started click "Launch Stack" in your region of choice (for the tutorial you must use us-east-1):

| Region       | Stack                                                                                                                                                                                                                                                                                                              |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| us-east-1 (N. Virginia) **Tutorial Region**    | {{% button href="https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=[Yourname]-Spack-Cluster&templateURL=https://notearshpc-quickstart.s3.amazonaws.com/0.1.0/cfn.yaml&param_ConfigS3URI=https://notearshpc-quickstart.s3.amazonaws.com/0.1.0/cfn.yaml&param_EnableBudget=false" icon="fas fa-rocket" %}}Launch Stack{{% /button %}} |
| Oregon (us-west-2)    | {{% button href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/review?stackName=[Yourname]-Spack-Cluster&templateURL=https://notearshpc-quickstart.s3.amazonaws.com/0.1.0/cfn.yaml&param_ConfigS3URI=https://notearshpc-quickstart.s3.amazonaws.com/0.1.0/cfn.yaml&param_EnableBudget=false" icon="fas fa-rocket" %}}Launch Stack{{% /button %}} |
| Ireland (eu-west-1)   | {{% button href="https://eu-west-1.console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/review?stackName=[Yourname]-Spack-Cluster&templateURL=https://notearshpc-quickstart.s3.amazonaws.com/0.1.0/cfn.yaml&param_ConfigS3URI=https://notearshpc-quickstart.s3.amazonaws.com/0.1.0/cfn.yaml&param_EnableBudget=false" icon="fas fa-rocket" %}}Launch Stack{{% /button %}} |
| Frankfurt (eu-central-1) | {{% button href="https://eu-central-1.console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/create/review?stackName=[Yourname]-Spack-Cluster&templateURL=https://notearshpc-quickstart.s3.amazonaws.com/0.1.0/cfn.yaml&param_ConfigS3URI=https://notearshpc-quickstart.s3.amazonaws.com/0.1.0/cfn.yaml&param_EnableBudget=false" icon="fas fa-rocket" %}}Launch Stack{{% /button %}} |

### Step 2
In [Account Setup](/00-account-setup.html) we authenticated with the AWS Console. If you are running this tutorial on your own (not in an AWS run event), you'll be prompted to login to the AWS Management Console at this point. 

### Step 3
On the next page you'll be presented with a bunch of parameters, most of which we'll leave as default. Change the following parameters:

1. Change the name of the stack from `[Yourname]-Spack-Cluster` to i.e. `Sean-Spack-Cluster`
2. Fill out the `UserPasswordParameter` with a temporary password. Keep it simple! You will be prompted to change it on first use.
3. Select `I acknowledge that AWS Cloudformation might create IAM resources`.
4. Now, click `Create Stack` to deploy the QuickStart Environment.

![CloudFormation Create Stack](/images/create_stack.png)

### Deployment
Deployment takes about 15 minutes. This QuickStart provisions:

* A Cloud9 Integrated Development Environment (IDE) in the selected region;
* an AWS Parallel Cluster environment, named `hpc-cluster`

Go get a cup of ☕️ while waiting for the stacks to complete.

Provisioning is complete when all Stacks show `CREATE_COMPLETE`.

![CloudFormation CREATE_COMPLETE](/images/cfn_console.png)

### Access Cluster

When all stacks show `CREATE_COMPLETE`, click on the Outputs tab of the `[Yourname]-Spack-Cluster` stack. 

Click on the `ResearchWorkspaceURL` to go directly to the Cloud9 Instance that was created. 

![CloudFormation Outputs Tab](/images/cfn_output.png)
