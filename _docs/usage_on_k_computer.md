---
title: "Using GENESIS on K Computer"
excerpt: "How to use GENESIS on RIKEN's last generation super computer K."
last_modified_at: 2025-05-14T11:59:26+09:00
layout: single
toc: false
---


The followings are recommended installation scheme, an example of the
batch script file (`run.sh`) for hybrid MPI/OpenMP computation with 64
MPI processes and 4 OpenMP threads (256 CPU cores in total), and a
command to submit a job. Please see also the user guide of the computer
system.

## Installation

    $ cd /home/user/genesis
    $ ./configure --host=k
    $ make
    $ make install

 Note that in K computer and other
FX10/FX100 machines, `prst_setup` (Parallel I/O tool) is not installed,
because Fujitsu compilers are not suitable for compiling the program.

##  Batch script

    #!/bin/bash -x
    #PJM --rsc-list "elapse=24:00:00"
    #PJM --rsc-list "node=32"
    #PJM --mpi "proc=64"
    #PJM --stg-transfiles all
    #PJM --stgin  "./spdyn ./"
    #PJM --stgin  "./input.pdb ./"
    #PJM --stgin  "./input.psf ./"
    #PJM --stgin  "./par_all36_prot_lipid.prm ./"
    #PJM --stgin  "./top_all36_prot_lipid.rtf ./"
    #PJM --stgin  "./INP2 ./"
    #PJM --stgin  "./md1.rst ./ confirm=no"
    #PJM --stgout "./md2.dcd /data/hp??????/MD/results/dcd/" # "hp??????" is your project id"
    #PJM --stgout "./md2.rst ./"
    #PJM -s

    . /work/system/Env_base
    export OMP_NUM_THREADS=4
    mpiexec ./spdyn INP2

##  Usage

    # Execute run1.sh to run3.sh sequentially
    $ pjsub --step run1.sh
    $ pjsub --step --sparam "jid=XXXXXXX" run2.sh
    $ pjsub --step --sparam "jid=XXXXXXX" run3.sh

    # Check running jobs
    $ pjsub -E

    # Delete a job
    $ pjdel JOB_ID

