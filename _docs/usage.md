---
title: "Using GENESIS"
excerpt: "How to use GENESIS."
last_modified_at: 2025-05-14T11:59:26+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---


In the GENESIS program package, not only MD simulators but also various
trajectory analysis tools are available. The users execute those
programs on command line. Here, we briefly explain how to execute them.
The users may find other useful information in the online
[Tutorials](/docs/tutorial_2022/) for practical usage, and also user
manual for basic usage (see Chapters "Getting started" and "Available
programs").

## General usage of the MD simulators (atdyn/spdyn) 

The GENESIS program package contains two MD simulators: atdyn and spdyn.
In most cases, atdyn is applied to small systems or coarse-grained
systems. On the other hand, spdyn is mainly employed for large systems
(> 100,000 atoms) in a periodic boundary condition. Basic usage of
atdyn and spdyn is almost same, and it is displayed by executing the
program with the "`-h`" option.

```bash
# Show the usage of spdyn 
$ /home/user/genesis/bin/spdyn -h
```

Template input (or control) file of atdyn/spdyn can be obtained with the "`-h
ctrl`" or "`-h ctrl_all`" option. If the users want to use spdyn for MD
simulations, the following command is executed to generate a template control
file:

```bash
# Generate a template control file for MD 
$ /home/user/genesis/bin/spdyn -h ctrl_all md > INP
$ less INP
```

Then, the users edit this control file by themselves according to the user's
desired simulation condition. Note that in one line of the file all characters
after "#" are recognized as a comment (not read in the program). Example control
files are shown in [Examples](/docs/examples/) and
[Tutorials](/docs/tutorial_2022/). In order to run the MD
simulation, the users execute atdyn/spdyn for the control file. In the following
example, MD simulation is carried out with 16 CPU cores, where the 4 MPI
processors and 4 OpenMP threads are employed for parallel calculation:

```bash
# Run spdyn for INP using 16 CPU cores with mpirun
$ export OMP_NUM_THREADS=4
$ mpirun -np 4 /home/user/genesis/bin/spdyn INP > log
```

You may want to know [more details about using spdyn with hybrid MPI/OpenMP](/docs/usage_spdyn_hybrid_MPI_OpenMP/).

##  General usage of the trajectory analysis tools

The GENESIS program package contains various trajectory analysis tools
(e.g., `rmsd_analysis` for the RMSD calculation, `trj_analysis` for the
distance calculation, and so on).  Basic usage of these tools is similar
to that in the MD simulators, and it is displayed by executing the
program with the "`-h`" option.  For example, in the case of
rmsd_analysis, the following command is executed:

```bash
# Generate a template control file of rmsd_analysis
$ /home/user/genesis/bin/rmsd_analysis -h ctrl > INP
$ less INP
```

Then, the users edit this control file by themselves according to the
user's desired condition. Example control files are shown in
[Examples](/docs/examples/) and
[Tutorials](/docs/tutorial_2022/). In order to run the analysis, the
users execute the tool for the control file.

```bash
# Run RMSD analysis
$ /home/user/genesis/bin/rmsd_analysis INP > log
```

Note that MPI is disable in most analysis tools, while OpenMP parallelization is
enable in some tools (e.g., `wham_analysis` and `mbar_analysis`).

Sometimes you may prefer to [analyze multiple trajectories in a single run](/docs/usage_analysis_multiple_DCD/).

## Usage on specific computer systems

Here, we provide more detailed examples of the installation scheme of
GENESIS, batch scripts for Hybrid MPI/OpenMP computation, GPGPU
computation, and a command to submit a job for the following computer
systems.

- [Installation/usage on GPU workstations](/docs/usage_on_gpu_workstations)
- [Installation/usage on Intel PC cluster machines with/without GPU](/docs/usage_on_cluster_machines/)
- [Installation/usage on Fugaku at RIKEN R-CCS](/tutorials/genesis_tutorial_appendix_3_2022/)


<!--
## Old systems

-   [Installation/usage on HOKUSAI GreatWave (FX100) and BigWaterfall at RIKEN ACCC](/docs/usage_on_hokusai/)
-   [Installation/usage on K-computer at RIKEN R-CCS](/docs/usage_on_k_computer/)
-   [Installation/usage on TSUBAME2.5 at Tokyo Institute of Technology](/docs/usage_on_tsubame/)
-->
