+++
title = "c. Initial Setup"
date = 2019-09-18T10:46:30-04:00
weight = 30
tags = ["tutorial", "spack", "cloud9"]
+++

### Step 1
First login to the [AWS Console](https://console.aws.amazon.com/) using the steps given in [Account Setup](/00-account-setup.md). If you are running this tutorial on your own (not in an AWS run event), you'll need to login to the AWS Management Console at this point. 

### Step 2

First we're taken to the AWS Console homepage. For the sake of this tutorial we're using the **N. Virginia (us-east-1)** region. Confirm that aligns with the region shown in the upper right hand corner.

![AWS Console](/images/aws_console.png)

### Step 3

Next navigate to the Cloud9 Console by searching for **Cloud9** in the search bar, or choose **Services**, then **Cloud9**.

![Cloud9 Navigate](/images/cloud9_navigate.png)

{{%expand "💡Pro Tip: Pin Cloud9 (Click to expand)" %}}
You can pin the Cloud9 link to the Top bar of your AWS console. Just click the <i class="fa fa-thumbtack" aria-hidden="true"></i> icon in the upper bar and drag the Cloud9 Logo into the bar.
![Pin Cloud9](/images/drag_cloud9.png)
{{% /expand%}}


#### Step 4: Create Cloud9 IDE

1. Choose **Create Environment**
2. Name your environment **[Yourname]-Spack-Tutorial** and choose **Next Step**.
![Cloud 9](/images/introductory-steps/cloud9-name.png)
3. On the **Configure Settings** page, change 
* **Instance Type** to `m5.large`
* **Platform** to `Ubuntu 18.04`
![Cloud 9](/images/cloud9_setup.png)
4. Choose **Next Step**.
5. Choose **Create Environment**.

Your AWS Cloud9 instance will be ready in a few minutes!

Now is a perfect time to grab a cup of ☕️

![Cloud9 Create](/images/introductory-steps/cloud9-create.png)