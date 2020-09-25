+++
title = "a. Build Binaries"
date = 2020-07-30T10:46:30-04:00
weight = 30
tags = ["tutorial", "spack", "fsx", "parallelcluster"]
+++

<!-- TODO Write description -->

## Step 1: Connect to the cluster

<!-- TODO -->

```bash
pcluster ssh spack
```

Once on the cluster we can see there's a Lustre partition mounted at `/spack`:

```bash
$ df -h
Filesystem                Size  Used Avail Use% Mounted on
udev                      7.6G     0  7.6G   0% /dev
tmpfs                     1.6G  1.2M  1.6G   1% /run
/dev/nvme0n1p1             49G   15G   35G  30% /
tmpfs                     7.6G     0  7.6G   0% /dev/shm
tmpfs                     5.0M     0  5.0M   0% /run/lock
tmpfs                     7.6G     0  7.6G   0% /sys/fs/cgroup
/dev/loop1                 29M   29M     0 100% /snap/amazon-ssm-agent/2012
/dev/loop0                 97M   97M     0 100% /snap/core/9436
tmpfs                     1.6G   20K  1.6G   1% /run/user/128
/dev/loop2                 97M   97M     0 100% /snap/core/9665
/dev/nvme1n1              196G   61M  186G   1% /shared
10.0.1.240@tcp:/a47ufbmv  1.1T  944M  1.1T   1% /spack
tmpfs                     1.6G     0  1.6G   0% /run/user/1000
```

## Step 2: Verify Spack

<!-- TODO -->

Next we verify spack can install packages to the correct location, for example if install a small package **zlib**:

```bash
$ spack install zlib
$ spack find zlib
```

We'll see that zlib is installed at the spack root, which is `/spack/opt`. We can also see that the path includes platform **Ubuntu 18.04**, architecture **Skylake**, and compiler **GCC 7.5.0** information. We'll see how this plays out when we build a binary mirror later.

![zlib install](/images/binary_mirror/zlib.png)

## Step 2: Launch a distributed build

Zlib is a small package and only took 4 seconds to install. If we install a larger package, such as **openfoam**, building from source can take an hour or longer. In order to speed this up, we're going to use the scheduler to distribute the build over 3 instances.

Each instance has 36 physical cores, so our build fleet has a total of 108 cores. The build is coordinated through the Lustre filesystem.

<!-- TODO Add description on how distributed builds work -->

```bash
cat > build_openfoam.sbatch << EOF
#!/bin/bash
#SBATCH --job-name=spack-mirror-create
#SBATCH --ntasks=3 --nodes=3
#SBATCH --output=spack.out

srun spack install -j 36 openfoam
EOF

sbatch build_openfoam.sbatch
watch squeue
```
Once this job is submitted, we see that the job goes into pending `PD` state. T 
```bash
Every 2.0s: squeue                                                                                ip-10-0-2-91: Thu Aug  6 17:12:04 2020

             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
                 2   compute spack-mi   ubuntu PD       0:00      3 (Nodes required for job are DOWN DRAINED or reserved for jobs in higher priority partitions)
```

Once the instances are launched the job will go into running `R` state. At that point we can run `watch --color spack find` to see the packages being installed in real time:

```bash
$ watch --color spack find
-- linux-ubuntu18.04-skylake_avx512 / gcc@7.5.0 -----------------
autoconf-archive@2019.01.06
diffutils@3.7
libfabric@1.10.1
libffi@3.3
libiconv@1.16
libpng@1.6.37
libsigsegv@2.12
libsodium@1.0.18
libzmq@4.3.2
lz4@1.9.2
miniconda3@4.8.2
openmpi@4.0.3
pkgconf@1.7.3
tar@1.32
xz@5.2.5
zlib@1.2.11
zstd@1.4.5
```

Once this job is completed (takes about 50 minutes), we can see the runtime using Slurm accounting. For example if the job id is `3`, I can run:

```bash
$ sacct -j 3 -o jobid,jobname,alloccpus,state,exitcode,elapsed
      JobID    JobName  AllocCPUS      State  ExitCode    Elapsed 
------------ ---------- ---------- ---------- -------- ---------- 
3            spack-mir+          0  COMPLETED      0:0   00:53:03 
3.0               spack          0  COMPLETED      0:0   00:53:03 
```

This took 53 minutes to complete.

<!-- TODO add runtime & comparison with single node -->

## Step 3: Create a GPG Key

To build the build-cache we'll need to create a GPG key. Spack has wrappers to create this key and tell spack about it:

```
$ spack gpg init
$ spack gpg create [name] [email]
gpg: key BE9C17188227E13C marked as ultimately trusted
gpg: directory '/spack/opt/spack/gpg/openpgp-revocs.d' created
gpg: revocation certificate stored as '/spack/opt/spack/gpg/openpgp-revocs.d/25B36BF16F7CA79FD9E74E34BE9C17188227E13C.rev'
```

## Step 4: Create Build Cache out the packages

In step 2 we installed the **openfoam** binaries. To see where those binaries were installed, let's run `spack find -p openfoam`:

```bash
$ spack find -p openfoam
==> 1 installed package
-- linux-ubuntu18.04-skylake_avx512 / gcc@7.5.0 -----------------
openfoam@2006  /spack/opt/spack/linux-ubuntu18.04-skylake_avx512/gcc-7.5.0/openfoam-2006-6nhutqc36knzbwxdcdbwaw44anhpx4nt
```
If we examine the directory `/spack/opt/spack/linux-ubuntu18.04-skylake_avx512/gcc-7.5.0/`, we'll see all of the dependency packages were installed there as well:

```bash
$ ls /spack/opt/spack/linux-ubuntu18.04-skylake_avx512/gcc-7.5.0/
gdbm-1.18.1-sz3iurwmyrbprtsy2hwqiqalwe3mi7b3                  snappy-1.1.7-dj2fcxmtmnwm6nqcsuc7i7wkg22fsmor
gettext-0.20.2-zzesk2v5zxu6xfisixtvpfp2uek4iyat               sz-2.0.2.0-dq377g6dq7hf2vxbmaa6atwot5bnfhid
...
```

Now we need to tell spack which files to upload to the build cache. Since we want to create this build cache for **openfoam** we'll use the spec of that. Spec is just a fancy term for package dependencies & version information for those packages:

```
$ spack spec openfoam
```
Now let's write it to a yaml file:

```bash
$ spack spec --yaml openfoam > openfoam_spec.yaml
```

Now let's create a build-cache out of that spec:

<!-- TODO -->
```bash
$ spack buildcache create -d /spack/ openfoam

spack buildcache create -a -u -d /spack --only package --rebuild-index -f openfoam
```

Now let's examine the directory structure that was created:

```
$ ls /spack/build_cache
```