---
title: "Frequently Asked Questions"
excerpt: "Answers to common questions about using GENESIS."
last_modified_at: 2025-05-13T11:59:26+09:00
layout: single
toc: false
sidebar:
  nav: sidebar-basic
---

## Q1: What is GENESIS and what is its main purpose? 

**A**: GENESIS (**GENeralized-Ensemble SImulation System**) is a suite of
computer programs designed for conducting molecular dynamics (MD) simulations of
biomolecular systems.  Developed by the Computational Biophysics Research Team
at RIKEN Center for Computational Science, it serves as an important research
tool in structural and molecular biology, enabling simulations of biomolecules
such as proteins, nucleic acids, lipids, and glycans.


## Q2: What are the key features and capabilities of GENESIS? 

**A**: GENESIS is a highly-parallel and multi-scale MD simulator that includes
enhanced sampling algorithms for biomolecular and cellular simulations. It
supports quantum mechanics / molecular mechanics (QM/MM), atomistic force fields
(like CHARMM and AMBER) and various coarse-grained (CG) models. Its capabilities
span basic MD simulations, energy minimization, and a wide range of advanced
sampling methods, including Replica-Exchange Molecular Dynamics (REMD),
Generalized replica-exchange with solute tempering (gREST), replica-exchange
umbrella sampling (REUS), multi-dimensional REMD method, Gaussian accelerated
Molecular Dynamics (GaMD), Reaction Path Search (RPATH), and Cryo-EM flexible
fitting. GENESIS is optimized for high performance on modern supercomputers,
utilizing hybrid MPI+OpenMP parallelism and GPGPU acceleration, and features
parallel I/O for extremely large systems.  Refer to [GENESIS
Functions](/docs/functions/) for more details.

## Q3: What are the system requirements for installing GENESIS? 

**A**: To install GENESIS, you need a Fortran compiler (such as `gfortran`,
`ifort`, or `frtpx`) and a preprocessor (`cpp`, `fpp`, or `frtpx`). For parallel
computing, MPI libraries (like OpenMPI, Intel MPI, or Fujitsu MPI) are
essential, while OpenMP is generally supported by modern Fortran compilers.
GENESIS can be installed on Linux, macOS, and Windows 10/11 operating systems.
A full list of requirements can be found
[here](/docs/installation/#requirements).

## Q4: Where can I find detailed installation instructions and user documentation? 

**A**: Comprehensive installation steps, including system requirements and
troubleshooting tips, are provided in Chapter 2 of the GENESIS User Guide and
[this page](/docs/installation/). The source code and regression test sets
are available for download from the [GENESIS repository on
Github](https://github.com/genesis-release-r-ccs/genesis). Additionally, online
tutorials for basic MD simulations, enhanced sampling, and various analyses can
be found on [this page](/docs/tutorial_2022/).  You can also generate a
template control file for any GENESIS program by executing the program with the
`-h ctrl` option (*e.g.*, `spdyn -h ctrl md`).

## Q5: How do I prepare input files for GENESIS simulations? 

**A**: To run MD simulations with GENESIS, you need to prepare input files that
define the initial structure's coordinates, system topology, and force field
parameters. GENESIS supports input file formats from various common MD software,
including CHARMM, AMBER, and GROMACS. Typically, essential input files consist
of a topology file (topfile, prmtopfile, or grotopfile), a parameter file
(parfile), and a structure/coordinate file (psffile, pdbfile, crdfile,
ambcrdfile, or grocrdfile). These files are usually generated using external
setup tools such as
[VMD/PSFGEN](https://www.ks.uiuc.edu/Research/vmd/plugins/psfgen/),
[CHARMM-GUI](https://www.charmm-gui.org/),
[LEaP](https://ambermd.org/tutorials/pengfei/index.php), or 
[SMOG/SMOG2](https://smog-server.org/) servers.  CG input files can be generated
using
[GENESIS-cg-tool](https://github.com/genesis-release-r-ccs/genesis_cg_tool).
Check our [Tutorials](/docs/tutorial_2022/) for more information.


## Q6: What analysis tools are available in GENESIS? 

**A**: GENESIS offers a comprehensive suite of trajectory analysis tools. These
include programs for analyzing center of mass coordinates (`comcrd_analysis`),
diffusion constant (`diffusion_analysis`), hydrogen bonds (`hb_analysis`),
membrane thickness (`lipidthick_analysis`), mean-square displacement (MSD)
(`msd_analysis`), RMSD (`rmsd_analysis`), and various other structural and
dynamic properties. It also provides tools for free energy analysis, such as
WHAM (`wham_analysis`) and MBAR (`mbar_analysis`).  There is also specialized
SPANA tools for large-scale parallel analyses.  Refer to the GENESIS User Guide
(Section 3.2 Analysis tools) for more details.


## Q7: Is GENESIS free software, and what are its citation requirements? 

**A**: Yes, GENESIS is free software distributed under the terms of the GNU
Lesser General Public License (LGPL) version 3 for versions 1.5 and later, and
GNU General Public License (GPL) version 2 for 1.4 and earlier versions.  Please
refer to [this page](/docs/citation/) for the requirements of citation.


## Q8: Are there any known limitations or common issues users should be aware of? 

**A**: As a relatively new MD simulation program, GENESIS may still contain
defects or bugs, so users are advised to carefully verify simulation results.
Common installation problems often stem from missing compilers, MPI libraries,
or mathematical libraries, or from incorrectly set environment paths. During
simulations, frequently encountered issues include:

- SHAKE algorithm convergence failures: often due to insufficient equilibration, problematic initial structures, or inappropriate input parameters.
- Atomic clashes: where atom pairs are too close, leading to numerical errors. This can occur in initial structures or due to periodic boundary conditions.
- Domain and cell definition issues in SPDYN: indicating that the number of MPI processors is unsuitable for the system size. Solutions often involve reducing the number of MPI processors, adjusting pairlistdist, or rebuilding a larger system.

Feel free to open a [GitHub
Issue](https://github.com/genesis-release-r-ccs/genesis/issues) for questions
and bug reports.