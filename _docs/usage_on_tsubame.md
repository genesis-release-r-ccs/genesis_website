---
title: "Using GENESIS on TSUBAME2.5 at Tokyo Institute of Technology"
excerpt: "How to use GENESIS on Tsubame."
last_modified_at: 2025-05-14T11:59:26+09:00
layout: single
toc: false
---


TSUBAME2.5 has 12 CPU cores and 3 GPU cards in each node. The followings
are recommended installation scheme, example of the batch script file
for hybrid MPI/OpenMP calculation using 24 nodes (144 MPI processors and
2 OpenMP threads with GPGPU calculation), and a command to submit a job.

## Installation 

    $ cd /home/user/genesis/src
    $ . /usr/apps.sp3/env/set_cuda-6.5.sh
    $ . /usr/apps.sp3/env/set_ompi-1.8.2_i2013.1.046_cuda6.5.sh
    $ ./configure --enable-gpu --enable-single --with-cuda='/usr/apps.sp3/cuda/6.5'
    $ make 
    $ make install

##  Batch script

Number of OpenMP threads and number of GPU cards are specified in
`run.sh` and `wrap.sh`, respectively.

Notice: GENESIS 1.1.2 or later may not require `wrap.sh`. You can omit
that script from the following sample.

`run.sh`:

    #!/bin/sh

    CUDADIR="/usr/apps.sp3/cuda/6.5"
    OMPIDIR="/usr/apps.sp3/mpi/openmpi/1.8.2/i2013.1.046_cuda6.5"
    BINDIR="/home/user/genesis/bin"

    export PATH="${OMPIDIR}/bin:${CUDADIR}/bin:${PATH}"
    export LD_LIBRARY_PATH="${CUDADIR}/lib64:${OMPIDIR}/lib:${LD_LIBRARY_PATH}"
    export OMP_NUM_THREADS=2
    cd ${PBS_O_WORKDIR}
    MPIPROCS=`wc -l $PBS_NODEFILE | awk '{print $1}'`

    mpirun -n $MPIPROCS -hostfile $PBS_NODEFILE -x OMP_NUM_THREADS=$OMP_NUM_THREADS -x PATH=$PATH -x LD_LIBRARY_PATH=$LD_LIBRARY_PATH ./wrap.sh $BINDIR/spdyn INP > log

`wrap.sh`:

    #!/bin/bash
    lr=${OMPI_COMM_WORLD_LOCAL_RANK:-0}
    gpuid=`expr ${lr} \% 3`
    export CUDA_VISIBLE_DEVICES=${gpuid}
    $@

##  Usage

```bash
# All scripts should be executable
$ chmod u+x run1.sh run2.sh
$ chmod u+x wrap.sh

# Execute run1.sh and run2.sh sequentially
$ t2sub -et 1 -q S -l select=24:mpiprocs=6:ncpus=12:gpus=3:mem=40gb -l walltime=24:00:00 -W group_list=t2g-hpxxxxxx run1.sh

$ t2sub -et 1 -q S -l select=24:mpiprocs=6:ncpus=12:gpus=3:mem=40gb -l walltime=24:00:00 -W group_list=t2g-hpxxxxxx -W depend=afterany:xxxxxxx.t2zpbsXX run2.sh

# Check running jobs
$ t2stat

# Delete a job 
$ t2del JOB_ID

# Check available CPU time
$ t2group
```

In the `t2sub` command
(`select=AAA:mpiprocs=BBB:ncpus=CCC:gpus=DDD:mem=EEE`),

`AAA:` Number of nodes\
`BBB:` Number of MPI processors in one node (should be
`12/OMP_NUM_THREADS`)\
`CCC:` Number of CPU cores in one node (should be always `12`)\
`DDD:` Number of GPU cards in one node (should be always `3`)\
`EEE:` Memory size

`CCC/BBB` should be `OMP_NUM_THREADS`, `group_list` is the group ID you
have, and `depend=afterany:` is the previous job ID. For details, please
see the user guide of TSUBAME2.5.

