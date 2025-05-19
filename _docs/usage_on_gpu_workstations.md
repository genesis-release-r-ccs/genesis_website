---
title: "Using GENESIS on GPU Workstations"
excerpt: "How to use GENESIS on GPU Workstations."
last_modified_at: 2025-05-14T11:59:26+09:00
layout: single
toc: false
sidebar:
  nav: sidebar-basic
---


Installation and usage of GENESIS on GPU machines are complicated.
The scheme strongly depends on the user's environment, and our
explanation described here may not work in your machine. If you have a
trouble in installing GENESIS, you may get a hint from
[here](usage-on-tsubame/index.html) to solve the problem.

####  CUDA setup 

You have to install [CUDA
Toolkit](https://developer.nvidia.com/cuda-toolkit) before GENESIS.
Official package by NVIDIA is available for various Linux distributions.
Please check
[here](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).

So, the `PATH` and `LIBRARY` paths are already set, and there is no need
to change `.bashrc`. But, if the `configure` command cannot find the
path automatically, you have to give the path through "`--with-cuda`" as
follows:

    $ ./configure --enable-single --enable-gpu --with-cuda=/opt/cuda

####  Setup and installation of GENESIS

Make sure that the path to CUDA libraries is specified in
`.bashrc` before
the compilation. Here, we assume that cuda libraries are installed in
`/usr/local/cuda-12.6/`.

    export PATH=/usr/local/cuda-12.6/bin:$PATH
    export LD_LIBRARY_PATH=/usr/local/cuda-12.6/lib64:/lib:$LD_LIBRARY_PATH

Status of the GPU cards is checked by the following command:

    $ nvidia-smi

For the installation, we specify the "`--enable-gpu`" option in the
configure command:

    $ cd /home/user/genesis
    $ ./configure --enable-single --enable-gpu
    $ make
    $ make install

#### Verify the installation

The users can verify the installation of GENESIS by using same "test
sets". Please add 'gpu' word as like the following commands in this
case.

    $ ./test.py "mpirun -np 8 /home/user/genesis-X.Y.Z/bin/spdyn" gpu

####  Usage 

After the job is submitted, the users are recommended to check whether
GPU and CPU are working correctly by using the `nvidia-smi` and `top`
commands, respectively. In the message displayed by the `nvidia-smi`
command, if `Pwr:Usage`, `Memory-Usage`, and `GPU-Util` show larger
values, and each MPI process is assigned to the corresponding GPU card
ID, the calculation may work fine.

[Important Notice: a GPU card will be assigned to a
process.]{style="color: #0000ff;"} Single MPI process cannot use
multiple GPU cards. On the other hand, multiple MPI processes can be
assigned to a single GPU card. (Total \# of processes \>= GPU cards
used) If there are 8 processes and 2 GPU cards (id: 0 or 1), 4 processes
will be assigned to GPU of id 0, and the other 4 processes will be
assigned to GPU of id 1.

When you use all available GPUs:

    $ export OMP_NUM_THREADS=2 
    $ mpirun -np 8 -cpus-per-proc 2 ./spdyn INP > log &

You don't need to add special settings for GPU.

When you use NOT all GPUs:

    $ export OMP_NUM_THREADS=2
    $ export CUDA_VISIBLE_DEVICES=0,2
    $ mpirun -np 8 -cpus-per-proc 2 ./spdyn INP > log &

You have to specify GPU devices by their IDs. The device IDs can be
checked by `deviceQuery` utility in CUDA samples or `nvidia-smi`
command. Occasionally, IDs in `deviceQuery` and `nvidia-smi` are
different. if you met problems in GPU IDs, please check the output of
`deviceQuery` utility because `deviceQuery` shall use the same IDs as
GENESIS.

 Please note that NVIDIA GPU cards with
compute capability \< 3.5 are automatically ignored in GENESIS 1.1.5 or
later. (On 1.1.4 or before, users should add special settings to ignore
GPUs with CC \< 3.5.) You may not need special settings when you use all
available GPUs.

 `deviceQuery` can be found in CUDA sample
directory (ex.` /usr/local/cuda/samples/1_Utilities/deviceQuery`). You
may need to compile the executable. (You may also need to copy it to
your directory before compilation.) Once successfully compiled, run it
and check the IDs. For example,

    $ ./deviceQuery | grep "^Device"
    Device 0: "Tesla K20c"
    Device 1: "Quadro K600"
    Device 2: "Tesla K20c"

#### Old information (GENESIS 1.1.0-1.1.4)

[Open]{#id680b38b2d7dfa .collapseomatic tabindex="0"
title="Open"}[Close]{#swap-id680b38b2d7dfa .colomat-swap
style="display:none;"}

##### Workstation with single GPU card

The following command is an example for hybrid MPI/OpenMP computation
with 8 MPI processors and 2 OpenMP threads.**\
**

    $ export OMP_NUM_THREADS=2 
    $ mpirun -np 8 -cpus-per-proc 2 ./spdyn INP > log &

##### Workstation with multiple GPU cards (use all GPU cards)

The method depends on GENESIS version.

------------------------------------------------------------------------

GENESIS 1.1.2 or later

You don't need any special settings in this case. (Of course you can
specify OpenMP thread number as usual. It is omitted just for simplicity
in the following example.)

    # Example
    $ mpirun -np 8 ./spdyn INP > log &

In GENESIS 1.1.2 or later, `spdyn` automatically detects available GPU
cards and uses them, where GPU id of [mod(*processid*,# *of processes*)
or (*process id*)%(*\# of processes*)]{style="color: #ff0000;"} will be
used by *processid*th process. If you wanna use only one of GPUs or GPUs
of specified ids, please check next section.

------------------------------------------------------------------------

GENESIS 1.1.1 or before

If you wanna use all GPU cards for single simulation, you need some
wrapper script, [`wrap.sh`]{style="color: #008000;"}, when running
`spdyn`. [This `wrap.sh` is available only for
OpenMPI.]{style="color: #008000;"} If you are using non-OpenMPI library
of MPI (such as IntelMPI), please update GENESIS version to 1.1.2 or
later. Otherwise, it is very hard to use all the GPU cards in a single
simulation.

    # Example
    $ export OMP_NUM_THREADS=2
    $ mpirun -np 8 -cpus-per-proc 2 ./wrap.sh ./spdyn INP > log &

This [`wrap.sh`]{style="color: #008000;"} script assigns each process to
a GPU. The following is the example for [two]{style="color: #ff0000;"}
GPU cards. If you have 3 or more GPUs, you must change the value colored
with red.

`wrap.sh`:

    #!/bin/bash
    lr=${OMPI_COMM_WORLD_LOCAL_RANK:-0}
    gpuid=`expr ${lr} \% 2`
    export CUDA_VISIBLE_DEVICES=${gpuid}
    $@

------------------------------------------------------------------------

##### Workstation with multiple GPU cards (use NOT all GPU cards)

 On GENESIS 1.1.5 or later, GPU cards with
compute capability \< 3.5 are automatically ignored.

You may have multiple GPU cards in a workstation. First, you need to
know the GPU IDs. Assuming that you have two Tesla cards for GPGPU
calculation and one Quadro card for Graphics Accelerator, you could find
their device IDs by using `deviceQuery`. `DeviceQuery` is available in
NVIDIA CUDA Samples. To compile `deviceQuery`, copy NVIDIA CUDA Sample
from CUDA installed directory (ex. `/usr/local/cuda/samples`) to your
directory, move to samples `/1_Utilities/deviceQuery`, and run `make`.
GPU device IDs can be easily obtained by just running nvidia-smi
command. You may use those IDs for first try. But IDs from `nvidia-smi`
and this `deviceQuery` are occasionally different. If you met a trouble
about GPU IDs,  please check `deviceQuery` output and use IDs of this
output.

    $ ./deviceQuery | grep "^Device"
    Device 0: "Tesla K20c"
    Device 1: "Quadro K600"
    Device 2: "Tesla K20c"

In this example, Device IDs of Tesla cards are 0 and 2. For a single
simulation on a Tesla GPU card, specify the GPU device ID using
`CUDA_VISIBLE_DEVICES` environment variable as follows:

    $ export CUDA_VISIBLE_DEVICES=0
    $ mpirun -np 8 ./spdyn INP > log &

For two independent simulations on two Tesla GPUs, specify the GPU
device ID to each run using `CUDA_VISIBLE_DEVICES`:

    $ export CUDA_VISIBLE_DEVICES=0
    $ mpirun -np 8 ./spdyn INP1 > log1 &

    $ export CUDA_VISIBLE_DEVICES=2
    $ mpirun -np 8 ./spdyn INP2 > log2 &

For a single simulation using two Tesla GPUs, you may use the command as
followed. (This is an example for hybrid MPI/OpenMP computation with two
GPU cards.) Please note that this method depends on your GENESIS
version.

------------------------------------------------------------------------

How to do in GENESIS 1.1.2 or later

```bash
$ export OMP_NUM_THREADS=2
$ export CUDA_VISIBLE_DEVICES=0,2
$ mpirun -np 8 -cpus-per-proc 2 ./spdyn INP > log &
```

CUDA_VISIBLE_DEVICES must contain GPU card IDs which are used in
computations. GENESIS 1.1.2 or later no longer requires `wrap.sh`.

How to do in GENESIS 1.1.0 or 1.1.1

    $ export OMP_NUM_THREADS=2
    $ mpirun -np 8 -cpus-per-proc 2 ./wrap.sh ./spdyn INP > log &

[[`wrap.sh`]{style="color: #ff0000;"} is a script (note that this is
different from the sample above) to separate 8 MPI processes into two
GPU cards:]{style="font-size: 12pt;"}

``` p1
#!/bin/sh
gpuids=(0 2)
ompi_rank=${OMPI_COMM_WORLD_LOCAL_RANK:-0}
gpuid=${gpuids[`expr ${ompi_rank} \% ${#gpuids[@]}`]}
export CUDA_VISIBLE_DEVICES=${gpuid}
$@
```

[GPU IDs you want to use are specified at line 2 ([numbers in
[blue]{style="color: #0000ff;"}]{style="color: #000000;"}, two Tesla K20
cards in this example) in
`wrap.sh`. ]{style="font-size: 12pt;"}`Nvidia-smi`[ command allows you
to check if each MPI process is properly assigned to the corresponding
GPU ID.]{style="font-size: 12pt;"}

It must be noted here that this [`wrap.sh`]{style="color: #ff0000;"} is
available only in OpenMPI environment. If you are using multiple GPUs on
non-OpenMPI environment (such as IntelMPI), please update GENESIS to
1.1.2 or later version. Otherwise, utilization of multiple GPUs might be
very tricky.

------------------------------------------------------------------------

You can check the activities of GPUs by `nvidia-smi` command.

    $ nvidia-smi
    Tue Jul 12 18:35:26 2016       
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 367.27                 Driver Version: 367.27                    |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |===============================+======================+======================|
    |   0  Quadro K600         Off  | 0000:03:00.0     Off |                  N/A |
    | 29%   58C    P8    N/A /  N/A |      0MiB /   980MiB |      0%      Default |
    +-------------------------------+----------------------+----------------------+
    |   1  Tesla K20c          Off  | 0000:04:00.0     Off |                    0 |
    | 32%   43C    P0    73W / 225W |    454MiB /  4742MiB |     52%      Default |
    +-------------------------------+----------------------+----------------------+
    |   2  Tesla K20c          Off  | 0000:21:00.0     Off |                    0 |
    | 35%   48C    P0    74W / 225W |    453MiB /  4742MiB |     47%      Default |
    +-------------------------------+----------------------+----------------------+
                                                                                  
    +-----------------------------------------------------------------------------+
    | Processes:                                                       GPU Memory |
    |  GPU       PID  Type  Process name                               Usage      |
    |=============================================================================|
    |    1     25390    C   ./spdyn                                        116MiB |
    |    1     25394    C   ./spdyn                                        113MiB |
    |    1     25398    C   ./spdyn                                        115MiB |
    |    1     25401    C   ./spdyn                                        105MiB |
    |    2     25392    C   ./spdyn                                        107MiB |
    |    2     25396    C   ./spdyn                                        113MiB |
    |    2     25400    C   ./spdyn                                        114MiB |
    |    2     25402    C   ./spdyn                                        114MiB |
    +-----------------------------------------------------------------------------+

 The IDs of GPUs displayed by `nvidia-smi`
are not always the same as those obtained from `deviceQuery`. In the
above case the ID of Quadro from `nvidia-smi` is different from that
from `deviceQuery`. You should use the GPU IDs from `deviceQuery` for
assigning them to `CUDA_VISIBLE_DEVICES`.

