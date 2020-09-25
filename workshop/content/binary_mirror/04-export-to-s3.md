+++
title = "a. Test Mirror"
date = 2020-07-30T10:46:30-04:00
weight = 40
tags = ["tutorial", "spack", "fsx", "parallelcluster"]
+++


## Step 1: Export to S3

Now that we've created the mirror locally we can export it to S3:

<!-- TODO -->

```bash
aws fsx create-data-repository-task \
    --file-system-id fs-0123456789abcdef0 \
    --type EXPORT_TO_REPOSITORY \
    --paths path1,path2/file1 \
    --report Enabled=true
```

## Step 2: Test it

Let's test that the mirror is setup correctly.

<!-- TODO -->

```bash
aws s3 ls s3://spack-tutorial-mirror
```

## Step 3: Uninstall


## Step 4: Add Mirror

```
spack add mirror s3://spack-tutorial-mirror
```