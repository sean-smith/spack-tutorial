+++
title = "c. Setup Libraries"
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

To do so, we created a file `packages.yaml` file that contains the following:

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