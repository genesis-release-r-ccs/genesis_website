---
title: "Using GENESIS on RIKEN Hokusai"
excerpt: "How to use GENESIS on RIKEN Hokusai."
last_modified_at: 2025-05-14T11:59:26+09:00
layout: single
toc: false
---


##  GreatWave MPC (FX100)

GreatWave MPC (GW-MPC) has 32 CPU cores in each node. The followings are
recommended installation scheme, example of the batch script file for
hybrid MPI/OpenMP computation with 64 MPI processors and 4 OpenMP
threads (256 CPU cores in total), and a command to submit a job. Please
see also the user guide of the computer system.

### Installation {#installation style="text-align: justify"}

    $ cd /home/user/genesis
    $ module load sparc
    $ ./configure --host=k
    $ make
    $ make install

 Do not forget "module load sparc".

 In K computer and other
FX10/FX100 machines, `prst_setup` (Parallel I/O tool) is not installed,
because Fujitsu compilers are not suitable for compiling the program.

###  Batch script

    #!/bin/bash
    #PJM -L rscunit=gwmpc
    #PJM -L rscgrp=batch
    #PJM -L node=8
    #PJM -L elapse=24:00:00
    #PJM -o log -j
    #PJM --mpi "proc=64"

    bindir=/home/user/genesis/bin
    export OMP_NUM_THREADS=4
    mpirun ${bindir}/spdyn INP

In this case, `node=8` is specified, because each node has 32 CPU cores
(64\*4/32=8).

###  Usage

    # Execute run1.sh to run3.sh sequentially
    $ pjsub --step run1.sh
    $ pjsub --step --sparam "jid=XXXXXXX" run2.sh
    $ pjsub --step --sparam "jid=XXXXXXX" run3.sh

    # Check running jobs
    $ pjsub -E

    # Delete a job
    $ pjdel JOB_ID

    # Check CPU time 
    $ listcpu

The job ID (`jid=XXXXXXX`) given at run1 is specified at run2 and run3.

 


##  BigWaterfall

BigWaterfall MPC (BW-MPC) has 40 CPU cores in each node. The followings
are recommended installation scheme, example of the batch script file
for hybrid MPI/OpenMP computation with 64 MPI processors and 5 OpenMP
threads (320 CPU cores in total), and a command to submit a job. Please
see also the user guide of the computer system.

### Installation {#installation-1 style="text-align: justify"}

    $ cd /home/user/genesis
    $ module load intel
    $ ./configure FC=mpiifort CC=mpiicc F77=mpiifort
    $ make
    $ make install

 Do not forget "module load intel".

###  Batch script

    #!/bin/sh
    #PJM -L rscunit=bwmpc
    #PJM -L rscgrp=batch
    #PJM -L vnode=8
    #PJM -L vnode-core=40
    #PJM -L elapse=24:00:00

    export OMP_NUM_THREADS=5
    bindir=/home/user/genesis/bin
    mpirun -np 64 -ppn 8 ${bindir}/spdyn INP > log

###  Usage

    # Execute run1.sh to run3.sh sequentially
    $ pjsub --step run1.sh run2.sh run3.sh

    # Check running jobs
    $ pjstat

    # Delete a job 
    $ pjdel JOB_ID

    # Check CPU time
    $ listcpu

 


##  GPU server (GWACSG) 

GPU server (GWACSG) has 24 CPU cores and 4 GPU cards in each node. The
followings are recommended installation scheme, example of the batch
script file for hybrid MPI/OpenMP computation with 32 MPI processors and
3 OpenMP threads + 16 GPU cards, and a command to submit a job. Please
see also the user guide of the computer system.

### Installation

    $ pjsub --interact -L rscunit=gwacsg -L vnode=1 -x gpu_per_vnode=1
    $ cd /home/user/genesis
    $ module load cuda/8.0
    $ autoreconf
    $ ./configure --enable-gpu --enable-single --with-cuda=/bwfefs/opt/x86_64/cuda/8.0/ FC=mpiifort CC=mpiicc
    $ make
    $ make install
    $ exit

 Compilation is carried out on a computational
node.

###  Batch script

    #!/bin/sh
    #PJM -L rscunit=gwacsg
    #PJM -L rscgrp=gpu
    #PJM -L vnode=4
    #PJM -L vnode-core=24
    #PJM -x gpu_per_vnode=4
    #PJM -L elapse=24:00:00
    #PJM -j

    export OMP_NUM_THREADS=3
    module load cuda/8.0
    mpirun -np 32 -ppn 8 gpurun /home/user/genesis/bin/spdyn INP > log

