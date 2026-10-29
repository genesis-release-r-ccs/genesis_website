---
title: "Using GENESIS"
excerpt: "How to use GENESIS."
last_modified_at: 2026-10-29T14:20:00+09:00
layout: single
toc: true
toc_sticky: true
sidebar:
  nav: sidebar-basic
---

- [GENESIS User Guide](/assets/fundamental/GENESIS_UserGuide_v2.2.0.pdf)

In the GENESIS program package, not only MD simulators but also various
trajectory analysis tools are available. The users execute those
programs on command line. Here, we briefly explain how to execute them.
The users may find other useful information in the online
[Tutorials](/docs/tutorial_2022/) for practical usage, and also user
manual for basic usage (see Chapters "Getting started" and "Available
programs").

## General usage of the MD simulators (atdyn/spdyn/cgdyn)

The GENESIS program package contains three MD simulators: `atdyn`, `spdyn`, and `cgdyn`:
- **atdyn**: Primarily applied to small systems, non-periodic simulations, or small-to-medium coarse-grained (CG) simulations.
- **spdyn**: Highly optimized for large all-atom systems (> 100,000 atoms) under periodic boundary conditions using spatial domain decomposition and GPU acceleration.
- **cgdyn**: Specifically designed for large-scale, residue-level coarse-grained MD simulations (such as biomolecular condensates, chromatin, and cellular-scale systems) with dynamic load balancing on massively parallel supercomputers.

Basic command-line usage across `atdyn`, `spdyn`, and `cgdyn` is largely identical. A summary of command-line options is displayed by running the binary with the "`-h`" option:

```bash
# Show the usage of spdyn (or atdyn / cgdyn)
$ /home/user/genesis/bin/spdyn -h
```

Template input (or control) files for `atdyn`, `spdyn`, or `cgdyn` can be obtained with the "`-h ctrl`" or "`-h ctrl_all`" option. For example, the following command generates a template control file for MD simulations:

```bash
# Generate a template control file for MD 
$ /home/user/genesis/bin/spdyn -h ctrl_all md > INP
$ less INP
```

Then, the users edit this control file according to their desired simulation condition. Note that in any line of the file, all characters after "#" are treated as comments. Example control files are shown in [Examples](/docs/examples/) and [Tutorials](/docs/tutorial_2022/).

In order to run the MD simulation, the users execute `atdyn`, `spdyn`, or `cgdyn` with the control file. In the following example, an MD simulation is carried out using 16 CPU cores, where 8 MPI processes and 2 OpenMP threads are employed:

```bash
# Run spdyn (or atdyn / cgdyn) for INP using 16 CPU cores with mpirun
$ export OMP_NUM_THREADS=2
$ mpirun -np 8 /home/user/genesis/bin/spdyn INP > log
```

For `spdyn`, it is recommended to use at least 8 MPI processes (\\(\ge 8\\)) to achieve efficient spatial domain decomposition. 

For `cgdyn`, the number of MPI processes must be at least 8 (\\(\ge 8\\)) and a power of 2 (e.g., 8, 16, 32, ...). Specifying domain partitioning (`domain_x`, `domain_y`, `domain_z`) in the `[BOUNDARY]` section is optional; if explicitly specified, their product (\\(N_x \times N_y \times N_z\\)) must strictly equal the total number of MPI processes.

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
