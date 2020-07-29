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