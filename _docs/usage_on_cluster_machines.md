---
title: "Using GENESIS on Intel PC Clusters"
excerpt: "How to use GENESIS on PC clusters."
last_modified_at: 2025-05-14T11:59:26+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---


In this page, we show an example of the usage of GENESIS on Intel PC cluster
machines. Because the actual usage depends on the machine environment, the
following protocol may not be directly applicable to your system. Here, we
assume that the machine's IP address is 192.168.1.2, each computational node has
16 CPU cores (2 CPUs and 8 cores in one CPU), gridengine or its variant is
installed as a job scheduler, and the user's MPI environment is set by
`mpi-selector-menu`.

## Without GPGPU

The followings are example of the installation scheme, batch script file
for hybrid MPI/OpenMP computation with 16 MPI processes and 4 OpenMP
threads (64 CPU cores in total), and a command to submit a job.

### Installation

```bash
# Login the machine
$ ssh 192.168.1.2

# select one proper option (e.g., ib-openmpi-1.10.1_intel-15.0.4_cuda-6.5)
$ mpi-selector-menu

# login again to update MPI setting
$ exit
$ ssh 192.168.1.2

# install genesis
$ cd /swork/user/genesis
$ ./configure
$ make
$ make install
```

###  Batch script

#### Example 1 (Recommended):

```bash
#$ -S /bin/bash
#$ -cwd
#$ -pe ompi 64
#$ -V
#$ -q nogpu.q
BINDIR=/swork/user/genesis/bin
mpirun -machinefile $TMP/machines -np 16 -npernode 4 -npersocket 2 -x OMP_NUM_THREADS=4 ${BINDIR}/spdyn INP > md.log
```

If "`-npernode X`" does not work, use "`--bind-to socket`".


#### Example 2:

```bash
#$ -S /bin/bash
#$ -cwd
#$ -pe ompi 64
#$ -V
#$ -q nogpu.q
export OMP_NUM_THREADS=4
BINDIR=/swork/user/genesis/bin
mpirun -machinefile $TMP/machines -np 16 -npernode 4 -npersocket 2 ${BINDIR}/spdyn INP > md.log
```

<!--
In the batch script,

[red]{style="color: #ff0000;"}: Total number of CPU cores to be used\
[orange]{style="color: #ff6600;"}: Number of OpenMP threads\
[blue]{style="color: #0000ff;"}: Number of MPI processors\
[green]{style="color: #008000;"}: Number of MPI processors in each node\
[magenta]{style="color: #ff00ff;"}: Number of MPI processors in one CPU\
[purple]{style="color: #800080;"}: queue name

and relationships between these values are

[red]{style="color: #ff0000;"} = [orange]{style="color: #ff6600;"} \*
[blue]{style="color: #0000ff;"}\
total number of CPU cores in one node =
[orange]{style="color: #ff6600;"} \* [green]{style="color: #008000;"}\
total number of CPU cores in one CPU = [orange]{style="color: #ff6600;"}
\* [magenta]{style="color: #ff00ff;"}
-->

###  Usage

```bash
# Execute run1.sh to run3.sh sequentially
$ qsub -N R1 run1.sh
$ qsub -N R2 -hold_jid R1 run2.sh
$ qsub -N R3 -hold_jid R2 run3.sh

# Check running jobs
$ qstat -f

# delete a job
$ qdel JOB_ID
```

After submitting a job, the user should check whether the specified CPU
resources are fully utilized by running the `top` command in some of the
computational nodes. If the CPUs are not fully used, the option of
`mpirun` might not be appropriate.  In some cases, `--bind-to socket` may have
to be be used instead of `-npernode`.


## with GPGPU

The followings are example of the installation scheme and batch script
file for hybrid MPI/OpenMP computation with 16 MPI processes and 4
OpenMP threads with 2 GPU cards/node (64 CPU cores + 8 GPU cards in
total).

### Installation

```bash
# Login the machine
$ ssh 192.168.1.2

# select one proper option (e.g., ib-openmpi-1.10.1_intel-15.0.4_cuda-6.5)
$ mpi-selector-menu

# login again to update MPI setting
$ exit
$ ssh 192.168.1.2

# install genesis
$ cd /swork/user/genesis
$ ./configure --enable-gpu --enable-single
$ make
$ make install
```

### Batch script

#### GENESIS 1.1.2 or later

Number of MPI processors and OpenMP threads are specified in `run.sh`.

`run.sh`:

```bash
#$ -S /bin/bash
#$ -cwd
#$ -pe ompi 64
#$ -V
#$ -q gpu.q
export OMP_NUM_THREADS=4
BINDIR=/swork/user/genesis/bin
mpirun -machinefile $TMP/machines -np 16 -npernode 4 -npersocket 2 ${BINDIR}/spdyn INP > md.log
```

Number of GPU cards will be automatically detected. If you wanna exclude some
specific GPU card or wanna use only a single specific GPU card among multiple
cards, please check [this page](usage_on_gpu_workstations.md).

#### GENESIS 1.1.1 or before

Number of MPI processors and OpenMP threads are specified in `run.sh`,
and number of GPU cards are in `wrap.sh`.

`run.sh`:
```bash
#$ -S /bin/bash
#$ -cwd
#$ -pe ompi 64
#$ -V
#$ -q gpu.q
export OMP_NUM_THREADS=4
BINDIR=/swork/user/genesis/bin
mpirun -machinefile $TMP/machines -np 16 -npernode 4 -npersocket 2 ./wrap.sh ${BINDIR}/spdyn INP > md.log
```

<!--
In those batch scripts,

[red]{style="color: #ff0000;"}: Total number of CPU cores to be used\
[orange]{style="color: #ff6600;"}: Number of OpenMP threads\
[blue]{style="color: #0000ff;"}: Number of MPI processors\
[green]{style="color: #008000;"}: Number of MPI processors in each node\
[magenta]{style="color: #ff00ff;"}: Number of MPI processors in one CPU\
[purple]{style="color: #800080;"}: queue name

and relationships between these values are

[red]{style="color: #ff0000;"} = [orange]{style="color: #ff6600;"} \*
[blue]{style="color: #0000ff;"}\
total number of CPU cores in one node =
[orange]{style="color: #ff6600;"} \* [green]{style="color: #008000;"}\
total number of CPU cores in one CPU = [orange]{style="color: #ff6600;"}
\* [magenta]{style="color: #ff00ff;"}
-->

`wrap.sh`:

```bash
#!/bin/bash
lr=${OMPI_COMM_WORLD_LOCAL_RANK:-0}
gpuid=`expr ${lr} \% 2`
export CUDA_VISIBLE_DEVICES=${gpuid}
$@
```

<!--
where [red]{style="color: #ff0000;"} is the number of GPU cards
attached in one node.
-->

Usage of GPU nodes is same with no GPU case.  Please see the above explanation.
