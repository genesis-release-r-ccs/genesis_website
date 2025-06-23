---
title: "Hybrid MPI/OpenMP calculation with spdyn"
excerpt: "How to run GENESIS spdyn with hybrid MPI/OpenMP."
last_modified_at: 2025-06-23T11:59:26+09:00
layout: single
classes: wide
toc: false
sidebar:
  nav: sidebar-basic
---

To achieve optimal performance with `spdyn`, it is recommended that users first
understand the basic parallelization scheme it employs. `spdyn` uses a domain
decomposition method, in which the entire simulation box is divided into
multiple domains based on the number of MPI processes. Each domain is further
subdivided into cells, whose sizes are automatically adjusted to be
approximately equal to or larger than the `pairlistdist` specified in the control
file. All domains and cells share the same shape and size.

Each MPI process is responsible for one domain, and communication of atomic
coordinates and forces occurs only between neighboring domains. Within each
domain, the computation of both bonded and non-bonded interactions is
parallelized using OpenMP. This design results in a hybrid MPI/OpenMP
parallelization, which is generally more efficient than flat MPI parallelization
on modern systems with multi-core CPUs.

Because MPI and OpenMP are optimized for distributed-memory and shared-memory
architectures, respectively, MPI is primarily used for inter-node
parallelization, while OpenMP is used for intra-node parallelization.

The following diagrams illustrate examples of hybrid MPI/OpenMP setups:
- In Figure (a), 8 MPI processes are used, each spawning 4 OpenMP threads (totaling 32 CPU cores).
- In Figure (b), 27 MPI processes are used, each with 2 OpenMP threads (totaling 54 CPU cores).

For simplicity, only the *XY*-plane is shown in these illustrations.

![](/assets/images/2016_05_domdec.png)


For Case (a), the following commands are used:

```bash
$ export OMP_NUM_THREADS=4
$ mpirun -np 8 /home/user/genesis/bin/spdyn INP > md.log
```

In the log file, the user can see information about how many MPI processors and
OpenMP threads are used for hybrid parallel calculation and how many domains and
cells are constructed for the system. For Case (a), the log message looks like
this:

```
Setup_Mpi_Md> Summary of Setup MPI
  number of MPI processes   =        8
  number of OpenMP threads  =        4
  total number of CPU cores =       32

...

Setup_Boundary_Cell> Set Variables for Boundary Condition
  domains (x,y,z) =          2         2         2
  ncells (x,y,z)  =          6         6         6
```

If “unexpected” number of MPI processors or OpenMP threads is shown, there might
be an error in your command or batch scripts.

The user can specify domain numbers explicitly in the control file (`domain_x`,
`domain_y`, and `domain_z` in the `[BOUNDARY]` section). For example, in Case
(b) the following parameters and commands are used:

```toml
[BOUNDARY]
type          = PBC    # [PBC]
domain_x      = 3      # number of domains in x dimension
domain_y      = 3      # number of domains in y dimension
domain_z      = 3      # number of domains in z dimension
```

Commands:
```bash
$ export OMP_NUM_THREADS=2 
$ mpirun -np 27 /home/user/genesis/bin/spdyn INP > md.log
```

Note that in this way total number of MPI processors must be identical to
`domain_x` × `domain_y` × `domain_z`. Otherwise, the simulation stops
immediately at the setup stage. If domain numbers are not specified in the
control file, they are automatically determined at the beginning of the
simulation according to the number of MPI processors. However, *this automatic
setting can work in the case MPI number is suitable to construct proper numbers
of domain and cell*. 

The user can set “`export OMP_NUM_THREADS=1`” in hybrid MPI/OpenMP calculation,
while it means flat MPI.  
{: .notice--info}


Due to specific algorithms implemented in spdyn (J. Jung et al., 2014 
[<i class="fas fa-link"></i>](https://onlinelibrary.wiley.com/doi/full/10.1002/jcc.23591)),
there is a strict rule for the number of cells in each domain, where **the domain
should be composed of more than 8 cells and at least 2 cells in each dimension**.
Thus, to use spdyn the target system should be large enough. The user should
remember this rule, especcially, in the NPT simulation, because the box size can
change during the simulation. In the NPT simulation, domain numbers are fixed to
the initial values, while cell numbers can change and are adjusted to keep the
cell size larger than `pairlistdist`. When the cell number in one dimension
unfortunately becomes one, the simulation stops immediately because of the
violation of the above rule. The user may often encounter this situation if the
cell size is very close to `pairlistdist` and the cell number in one dimension is
only two at the beginning of the MD simulation. To avoid such problems, the user
may have to use smaller number of MPI processors (which makes larger domain) or
shorter `pairlistdist` (making much cells in one domain), or reconstruct a larger
system.