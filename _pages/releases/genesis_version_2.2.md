---
permalink: /releases/genesis_version_2.2/
title: "GENESIS Version 2.2"
excerpt: "Release of GENESIS v2.2."
last_modified_at: 2025-05-08T11:59:26-09:00
toc: false
sidebar:
  title: "GENESIS Releases"
  nav: sidebar-releases
---

## List of functions in ver. 2.2.0

- CHARMM force fields

  - CHARMM19, 22, 27, C36, C36m

- AMBER force fields

  - FF99SB (ATDYN/SPDYN), FF14SB (ATDYN/SPDYN), FF19SB (SPDYN)

- SPICA/pSPICA model ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}

- Structure-based (Go-like) models for protein

  - Cα and KB Go models

  - All-atom Go model

  - AICG2+ protein model

  - Macro-mixing KB Go/DoME model

- Coarse-Grained (CG) models

  - 3SPN.2C DNA model

  - Structure-based RNA model

  - HPS/KH, CALVADOS, and Mpipi ![](/assets/images/2016_04_icon4neworange.gif){: width="40"} models for IDR and RNA

  - PWMcos model for protein-DNA sequence-specific interaction

  - iSoLFv2 model for lipid ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}

  - A tool for preparing CG files: GENESIS-CG-tool

- CGDYN: a new high-performance MD engine specifically for CG ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}

- Energy minimization

  - Steepest descent method

  - LBFGS

- Molecular dynamics simulation

  - Leapfrog, velocity Verlet, and RESPA integrators

  - NVE, NVT, NPT, NPAT, NPγT ensembles

  - Langevin (BAOAB with half-step pressure) ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}, Berendsen/Bussi/NHC thermostat with MTK-style barostat

  - Particle mesh Ewald method

  - Constraints (SHAKE, RATTLE, SETTLE, LINCS)

  - Restraints (Position, distance, angle, dihedral, PCA)

  - Simulated annealing MD

  - Steered MD and Targeted MD

- Implicit solvent model

  - GB/SA model with CHARMM and AMBER force fields

  - EEF1, IMM1, IMIC models

- Replica-exchange molecular dynamics method

  - Temperature, Pressure, Surface-tension REMD

  - Replica-exchange umbrella sampling (REUS or H-REMD)

  - Generalized replica exchange with solute tempering (gREST)

  - On-the-fly energy analysis in gREST

  - Multi-dimensional REMD (T-REMD/REUS, gREST/REUS)

  - Collective variable REUS

- Boundary

  - Periodic boundary condition (PBC)

  - Isolated system (no-PBC w/ or w/o spherical boundary potential)

- String method

- Gaussian accelerated molecular dynamics method (GaMD, GaREUS)

- Adaptively biased molecular dynamics and its variants ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}

  - Adaptively biased molecular dynamics (ABMD) ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}

  - Multiple-walker ABMD (MW-ABMD) ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}

  - Replica-exchange ABMD (RE-ABMD) ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}

  - Bias-exchange ABMD (BE-ABMD) ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
  
  - Generalized replica exchange with solute tempering ABMD (gREST-ABMD) ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}

- QM/MM calculation

  - Minimization and vibrational analysis

  - Anharmonic vibrational analysis ([SINDO](https://tms.riken.jp/en/research/software/sindo/))

  - QM/MM molecular dynamics

  - Minimum energy path search using String methods

  - Supported QM software: [Gaussian](http://gaussian.com/), [Q-Chem](http://www.q-chem.com), [TeraChem](http://www.petachem.com), [DFTB+](https://www.dftbplus.org), [QSimulate-QM](https://qsimulate.com/academic)

  - Available MM force field: CHARMM and AMBER

- Cryo-EM flexible fitting

- Free-energy perturbation method (FEP)

  - Dual and hybrid topology schemes

  - Soft core potentials for non-bonded interactions

  - Modified Hamiltonian

  - Lambda-exchange (Hamiltonian exchange) FEP

  - GPGPU acceleration

- Computation with working precision (Single/Double precision)

- GPGPU calculation in SPDYN

- TIP4P water model in SPDYN

- Trajectory analysis tools (Bond, Angle, Dihedral, RMSD, etc + 18 tools)

- Spatial decomposition analysis (SPANA)

- Morphing calculation(ATDYN)

## Change Log

- Oct 20, 2026: GENESIS 2.2.0 released

  - (ATDYN/SPDYN) Added adaptively biased molecular dynamics (ABMD) and its variants.

  - (CGDYN) Added as a new MD engine for coarse-grained simulations.

  - (ATDYN/CGDYN) Added Mpipi coarse-grained model for IDP and RNA.

  - (ATDYN) Added iSoLFv2 for coarse-grained lipid simulations.

  - (SPDYN) Added SPICA/pSPICA for explicit-water coarse-grained simulations.

  - (SPDYN) Added a new Langevin integrator based on the BAOAB scheme.

  - Added support for LLVM/Clang and NVIDIA compilers.

  - Added support for AMD GPUs.
