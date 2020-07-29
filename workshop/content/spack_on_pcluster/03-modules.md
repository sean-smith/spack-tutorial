+++
title = "d. Setup Libraries"
date = 2019-09-18T10:46:30-04:00
weight = 40
tags = ["tutorial", "spack"]
+++

AWS ParallelCluster comes pre-installed with **openmpi**, **intelmpi**, **slurm** and **libfabric**. These versions have built in support for our high-speed interconnect [Elastic Fabric Adapter](https://aws.amazon.com/hpc/efa/) (EFA). In this cluster we've added these modules to Spack. To see availible modules, run `module avail`:

```bash
$ module avail
--- /shared/spack-0.15/share/spack/modules/linux-ubuntu18.04-skylake_avx512 ----
miniconda3/4.8.2-gcc-7.5.0  

----------------- /usr/share/modules/modulefiles ----------------------
dot  libfabric-aws/1.10.1amzn1.1  module-git  module-info  modules  null  openmpi/4.0.3  use.own  

------------- /usr/share/modules/modulefiles ---------------------------
dot  libfabric-aws/1.10.1amzn1.1  module-git  module-info  modules  null  openmpi/4.0.3  use.own  

----------- /opt/intel/impi/2019.7.217/intel64/modulefiles --------------
intelmpi  
```

You can see we've added **openmpi**, **intelmpi**, **slurm** and **libfabric** as [external packages](https://spack.readthedocs.io/en/latest/build_settings.html#external-packages) to spack.

```bash
$ spack find
==> 13 installed packages
-- linux-ubuntu18.04-skylake_avx512 / gcc@7.5.0 -----------------
miniconda3@4.8.2

-- linux-ubuntu18.04-x86_64 / gcc@7.5.0 -------------------------
autoconf@2.69    gdbm@1.18.1      libtool@2.4.6  ncurses@6.2    patchelf@0.10  pkgconf@1.7.3
automake@1.16.2  libsigsegv@2.12  m4@1.4.18      openmpi@4.0.3  perl@5.30.3    readline@8.0
```

Note that packages don't show up in `spack find` until you install something that requires them. To see the external packages take a look at `/shared/spack-0.15/etc/spack/packages.yaml`:

```yaml
packages:
        openmpi:
                modules:
                        openmpi@4.0.3 fabrics=libfabric +pmi schedulers=slurm: openmpi/4.0.2
                buildable: True
        intelmpi:
                modules:
                        intelmpi@2019.7.217: intelmpi/2019.7.217
                buildable: True
        slurm:
                paths:
                        slurm@19.05.5: /opt/slurm/
                buildable: False
        libfabric:
                modules:
                        libfabric@1.10.1 fabrics=efa: libfabric/1.10.1amzn1.1
                buildable: True
```

You can see that we have an **openmpi** package installed. We're going to use that to install the **osu-micro-benchmarks**:

```bash
spack install osu-micro-benchmarks
```

You'll see it picks up the external package and uses that as the **mpi** version.

![OSU Microbenchmarks with Spack](/images/spack_on_pcluster/osu_openmpi.png)

Now that we have the **osu-micro-benchmarks** we can run it locally to verify the installation:

```bash
module load osu-micro-benchmarks openmpi
mpirun -np 2 osu_latency
```

Now we can submit a job and have it run on 2 compute nodes:

```bash
cat > c5n_osu_latency.sbatch << EOF
#!/bin/bash
#SBATCH --job-name=osu-latency-job
#SBATCH --ntasks=2 --nodes=2
#SBATCH --output=osu_latency.out

module load osu-micro-benchmarks openmpi
srun osu_latency
EOF

sbatch c5n_osu_latency.sbatch
watch squeue
```

This will take a few minutes to launch the instances then start running the job, until then you'll see:

```bash
$ watch squeue
Every 2.0s: squeue                                                                               ip-10-0-1-151: Wed Jul 29 17:23:01 2020

             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
                 4   compute osu-late   ubuntu PD       0:00      2 (Nodes required for job are DOWN, DRAINED or reserved for jobs in hi
gher priority partitions)
```

Once the job runs we can review the results in the `osu_latency.out`:

```bash
$ less osu_latency.out
# OSU MPI Latency Test v5.6.2
# Size          Latency (us)
0                      19.86
1                      19.93
2                      19.91
4                      19.92
8                      20.04
16                     20.50
32                     20.50
64                     20.58
128                    20.72
256                    21.30
512                    21.53
1024                   22.27
2048                   23.85
4096                   27.65
8192                   33.25
16384                  39.34
32768                  58.13
...
```