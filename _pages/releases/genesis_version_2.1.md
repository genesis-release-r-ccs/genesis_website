---
permalink: /releases/genesis_version_2.1/
title: "GENESIS Version 2.1"
excerpt: "Release of GENESIS v2.1."
last_modified_at: 2025-05-08T11:59:26-09:00
toc: false
sidebar:
  title: "GENESIS Releases"
  nav: sidebar-releases
---

## List of functions in ver. 2.1.5

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
    -   GB/SA model with CHARMM and AMBER force fields
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
        ([SINDO](https://tms.riken.jp/en/research/software/sindo/))
    -   QM/MM molecular dynamics
    -   Minimum energy path search using String methods
    -   Supported QM software:
        [Gaussian](http://gaussian.com/), 
        [Q-Chem](http://www.q-chem.com), 
        [TeraChem](http://www.petachem.com), 
        [DFTB+](https://www.dftbplus.org), 
        [QSimulate-QM](https://qsimulate.com/academic)
    -   Available MM force field: CHARMM and AMBER
-   Cryo-EM flexible fitting
-   Free-energy perturbation method (FEP) ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
    -   Dual and hybrid topology schemes
    -   Soft core potentials for non-bonded interactions
    -   Modified Hamiltonian
    -   Lambda-exchange (Hamiltonian exchange) FEP
    -   GPGPU acceleration
-   Computation with working precision (Single/Double precision)
-   GPGPU calculation in SPDYN
-   TIP4P water model in SPDYN
-   Trajectory analysis tools (Bond, Angle, Dihedral, RMSD, etc + 18
    tools)
-   Spatial decomposition analysis (SPANA)
-   Morphing calculation(ATDYN)


## Change Log

-   Aug 16, 2024: GENESIS 2.1.4 released
    -   Bugfix of reading psf file when reading an atomtype that begins
        with a non-alphabetic letter.
    -   Fix configure.ac for setting LDFLAGS.
    -   Fix configure.ac for setting CFLAGS/FFLAGS in autoconf \>=2.70
    -   Fix regression test scripts so that warnings are not generated.
-   Jun 10, 2024: GENESIS 2.1.3 released
    -   Bugfix of random number generator for CG simulation in atdyn.
    -   Enable of Intel Compilers (ifx/icx) from 2024. (Fix of error
        from fseeko64/ftello64.)
    -   Bugfix of CUDA code on recent GPU.
-   Dec 26, 2023: GENESIS 2.1.2 released
    -   Some flags (`!$omp simd`, `!$dir$`) have been removed.
    -   Auto-test scripts in regression_test directory have been fixed;
        now, relative path of the program binary can be typed.
    -   Old versions of Gfortran (4.4.7-6.X.Y) are not supported.
    -   Intel Fortran version 2021.7.1 is not supported.
    -   sasa_analysis in sp_analysis has been fixed to be available for
        multiple compilers and architectures.
    -   Some regression test sets of REMD and RPATH have been modified
        to be available for multiple compilers and architectures.
-   Oct 2, 2023: GENESIS 2.1.1 released
    -   Add energy_analysis for QM/MM calculation
    -   Enable prediodic CV in pmf_analysis
    -   (ATDYN/SPDYN) Update simd directive for Intel compiler version > 23.1
    -   (ATDYN) Increase output frequency
    -   (SPDYN) Update configure file for CUDA > 12.0
    -   (ATDYN) Added QM interface with ORCA
    -   (SPDYN) Bugfix: fix timing of transfer charge values to GPU card
        in gREST
    -   (SPDYN) Bugfix: Ryckaert-Bellemans dihedral energy sum
    -   (SPDYN) Bugfix: Virial term in amber bond correction was not
        written correctly and corrected.
    -   (SPDYN) Bugfix: error in water number count is fixed in TIP4P
        case.
    -   (ATDYN/SPDYN) BugFix: fix generation of random seed in replica
        simulations (REMD/RPATH)
    -   (ATDYN/SPDYN) BugFix: fix wrong file name in large number of
        replicas
-   Feb 20, 2023: GENESIS 2.1.0 released
    -   (SPDYN) Free-energy perturbation with modified Hamiltonian
    -   Updates of Intel Fortran's options for AMD CPU
    -   Updates of regression test to prevent potential errors
