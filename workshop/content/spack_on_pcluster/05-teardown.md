+++
title = "f. Delete"
date = 2019-09-18T10:46:30-04:00
weight = 50
tags = ["tutorial", "spack"]
+++

To un-install Spack (and all modules) from the cluster, simply remove the `/shared/spack-0.15` directory:

```bash
sudo su
rm -rf $SPACK_ROOT
rm -rf /etc/profile.d/spack.sh
```

### Delete the Cluster

To delete all the infrastructure, navigate to AWS CloudFormation, Select the `[Yourname]-Spack-Tutorial` stack and click **Delete**. Repeat for the **hpc-cluster** stack. **Note** you do not have to delete the **aws-cloud9-*** stack or any substacks. 

![delete no-tears-cluster](/images/delete_no-tears-cluster.png)

### Delete the S3 Bucket

Go to [S3 Console](https://console.aws.amazon.com/s3/home) and delete the S3 Buckets `aws-hpc-quickstart-cloudtraillogs...` and `aws-hpc-quickstart-datarepository....` To do so, you'll first have delete the contents of that bucket, then delete the bucket.

![delete S3 Bucket](/images/delete_s3.png)