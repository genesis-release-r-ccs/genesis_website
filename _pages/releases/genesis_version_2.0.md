---
permalink: /releases/genesis_version_2.0/
title: "GENESIS Version 2.0"
excerpt: "Release of GENESIS v2.0."
last_modified_at: 2025-05-13T11:59:26-09:00
toc: false
sidebar:
  title: "GENESIS Releases"
  nav: sidebar-releases
---

## Download ver. 2.0.3

The source code, test set, and user manual are available from
[GitHub](https://github.com/genesis-release-r-ccs/genesis.git).
Click the "`Code`" bottom and select "Download ZIP" in the page.


## List of functions in ver. 2.0.3

-   CHARMM force fields
    -   CHARMM19, 22, 27, C36, C36m
-   AMBER force fields
    -   FF99SB (ATDYN/SPDYN), FF14SB (ATDYN/SPDYN), FF19SB (SPDYN)
-   MARTINI model
-   Go models for protein\
    -   Cα and KB Go models
    -   All-atom Go model
    -   AICG2+ protein model
    -   Macro-mixing KB Go/DoME model
-   3SPN.2C DNA model
-   Structure-based RNA model
-   HPS/KH model for IDR and RNA
-   PWMcos model for protein-DNA sequence-specific interaction
-   A tool for preparing CG files: GENESIS-CG-tool
-   Energy minimization
    -   Steepest descent method
    -   LBFGS
-   Molecular dynamics simulation
    -   Leapfrog, velocity Verlet, and RESPA integrators
    -   NVE, NVT, NPT, NPAT, NPγT ensembles
    -   Langevin, Berendsen, and Bussi thermostat/barostat
    -   Particle mesh Ewald method
    -   Constraints (SHAKE, RATTLE, SETTLE, LINCS)
    -   Restraints (Position, distance, angle, dihedral, PCA)
    -   Simulated annealing MD
    -   Steered MD and Targeted MD
-   Implicit solvent model
    -   GB/SA model with CHARMM and[ AMBER force
        fields]{style="color: initial;"}
    -   EEF1, IMM1, IMIC models
-   Replica-exchange molecular dynamics method
    -   Temperature, Pressure, Surface-tension REMD
    -   Replica-exchange umbrella sampling (REUS or H-REMD)
    -   Generalized replica exchange with solute tempering (gREST)
    -   On-the-fly energy analysis in gREST
    -   Multi-dimensional REMD (T-REMD/REUS, gREST/REUS)
    -   Collective variable REUS
-   Boundary
    -   Periodic boundary condition (PBC)
    -   Isolated system (no-PBC w/ or w/o spherical boundary potential)
-   String method
-   Gaussian accelerated molecular dynamics method (GaMD, GaREUS)
-   QM/MM calculation
    -   Minimization and vibrational analysis
    -   Anharmonic vibrational analysis
        (+[SINDO](https://tms.riken.jp/en/research/software/sindo/))
    -   QM/MM molecular dynamics
    -   Minimum energy path search using String methods
    -   Supported QM software:
        [Gaussian](http://gaussian.com/),
        [Q-Chem](http://www.q-chem.com),
        [TeraChem](http://www.petachem.com),
        [DFTB+](https://www.dftbplus.org),
        [QSimulate-QM](https://qsimulate.com/academic).
    -   Available MM force field: CHARMM and AMBER
-   Cryo-EM flexible fitting
-   Computation with working precision (Single/Double precision)
-   GPGPU calculation in SPDYN
-   TIP4P water model in SPDYN
-   Trajectory analysis tools (Bond, Angle, Dihedral, RMSD, etc + 18
    tools)
-   Spatial decomposition analysis (SPANA)
-   Morphing calculation(ATDYN)

## Change log

-   Dec 27, 2022\
    -   (SPDYN) Fix solute list allocation in gREST (Bugfix)
-   Dec 20, 2022\
    -   (SPDYN) Fix local restraints (Bugfix)
-   Dec 19, 2022
    -   Fix regression tests for Fugaku
    -   (SPDYN) Fix local restraints (Adding Error check, bugfix)
    -   (SPDYN) Fix structure check


