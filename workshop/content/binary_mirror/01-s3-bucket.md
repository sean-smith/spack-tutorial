+++
title = "a. Create an S3 Bucket"
date = 2020-07-30T10:46:30-04:00
weight = 10
tags = ["tutorial", "spack", "s3", "mirror"]
+++

For this portion of the tutorial, I'm assuming you've followed the guide in [Workshop Initial Setup](01-cluster-setup.html) and have a Cloud9 instance with AWS ParallelCluster installed. 

## Step 1

Go to the [Cloud9 Console](https://console.aws.amazon.com/cloud9) and connect to the Cloud9 instance you setup called **ResearchWorkspace** by clicking **Open IDE**:

![Cloud9 Console](/images/binary_mirror/cloud9_instance.png)

## Step 2

First we're going to use the AWS CLI to create an S3 bucket. The AWS CLI is pre-installed on the Cloud9 instance, we can run:

```bash
$ aws --help
usage: aws [options] <command> <subcommand> [<subcommand> ...] [parameters]
To see help text, you can run:

  aws help
  aws <command> help
  aws <command> <subcommand> help
aws: error: the following arguments are required: command
```

Amazon Simple Storage Service (S3) is a block storage service. At it's center is the concept of buckets, which can be thought of like domain names, they're globally unique and follow a directory structure. For example, I can put an object in the bucket `spack-tutorial-mirror` at the `/mirror/binary` directory and then reference it via the uri `s3://spack-tutorial-mirror/mirror/binary`.

First we need to choose a bucket name. I'm going to choose `spack-tutorial-mirror`, but this bucket name is already taken (by me) so please choose another. Buckets are globally unique and names can't be changed after they're created so choose wisely.

To create the bucket, we simply run:

```bash
aws s3 mb s3://spack-tutorial-mirror
```

![S3 Bucket Creation](/images/binary_mirror/s3_mirror.png)

## Step 3

Verify that bucket has been created properly:

```bash
aws s3 ls s3://spack-tutorial-mirror
```

You should see no output, if instead I was to list a non-existant bucket, I'd see NoSuchBucket exception:

![S3 Bucket Creation](/images/binary_mirror/s3_bucket.png)