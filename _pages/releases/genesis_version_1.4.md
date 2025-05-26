---
permalink: /releases/genesis_version_1.4/
title: "GENESIS Version 1.4"
excerpt: "Release of GENESIS v1.4."
last_modified_at: 2025-05-13T11.49:26-09:00
toc: false
sidebar:
  title: "GENESIS Releases"
  nav: sidebar-releases
---

##  List of functions in ver. 1.4.0

-   CHARMM and AMBER force fields
-   MARTINI model
-   Go models (Cα, all-atom, and KB Go models)
-   Energy minimization
    -   Steepest descent method
    -   LBFGS ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
-   Molecular dynamics simulation
    -   Leapfrog, velocity Verlet, and RESPA integrators
    -   NVE, NVT, NPT, NPAT, NPγT ensembles
    -   Langevin, Berendsen, and Bussi thermostat/barostat
    -   Particle mesh Ewald method
    -   GB/SA implicit solvent model ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
    -   Constraints (SHAKE, RATTLE, SETTLE, LINCS)
    -   Restraints (Position, distance, angle, dihedral, PCA)
    -   Simulated annealing MD
    -   Steered MD and Targeted MD
-   Replica-exchange molecular dynamics method
    -   Temperature, Pressure, Surface-tension REMD
    -   Replica-exchange umbrella sampling (REUS or H-REMD)
    -   Generalized replica exchange with solute tempering (gREST)
    -   Multi-dimensional REMD (T-REMD/REUS, gREST/REUS)
    -   Collective variable REUS
-   String method
-   Gaussian accelerated molecular dynamics method (GaMD, GaREUS) ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
-   QM/MM calculation ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
-   Anharmonic vibrational analysis 
    (+[SINDO](https://tms.riken.jp/en/research/software/sindo/)) ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
-   Cryo-EM flexible fitting ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
-   Computation with working precision (Single/Double precision)
-   GPGPU calculation in SPDYN
-   TIP4P water model in SPDYN
-   Trajectory analysis tools (Bond, Angle, Dihedral, RMSD, etc + 18
    tools ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}

## Change log

-   Oct 31, 2019: GENESIS 1.4.0 released
    -   (ATDYN) Implicit solvent model (GBSA)
    -   (ATDYN) QM/MM calculation
    -   (ATDYN) Energy minimization (LBFGS, Micro-iteration)
    -   (ATDYN) Anharmonic vibrational analysis (+SINDO)
    -   (ATDYN) CHARMM C19
    -   (SPDYN) All-atom Go model
    -   (ATDYN, SPDYN) Cryo-EM flexible fitting
    -   (ATDYN, SPDYN) Gaussian accelerated MD
    -   (ATDYN, SPDYN) Repulsive distance restraint
    -   (ATDYN, SPDYN, Analysis) Distance-based atom selection
    -   (Analysis) Upgrade remd_convert, mbar_analysis, and
        wham_analysis
    -   (Analysis) Add trajectory analysis tools, mode analysis tools,
        converters, free energy analysis tools, interface, and other
        utilities

