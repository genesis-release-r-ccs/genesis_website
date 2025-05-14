---
permalink: /releases/genesis_version_1.7/
title: "GENESIS Version 1.7"
excerpt: "Release of GENESIS v1.7."
last_modified_at: 2025-05-13T11:59:26-09:00
toc: false
sidebar:
  title: "GENESIS Releases"
  nav: sidebar-releases
---

##  Download ver. 1.7.1 

( **TODO:** add a download link here? )

The license of GENESIS source code is [the GNU Lesser General Public License version 3](https://www.gnu.org/licenses/lgpl-3.0.en.html).

## List of functions in ver. 1.7.1 

-   CHARMM force fields
    -   CHARMM19, 22, 27, C36, C36m
-   AMBER force fields
    -   FF99SB (ATDYN/SPDYN), FF14SB (ATDYN/SPDYN), FF19SB
        (SPDYN) ![](/assets/images/2016_04_icon4neworange.gif)
-   MARTINI model
-   Go models for protein\
    -   Cα and KB Go models
    -   All-atom Go model
    -   AICG2+ protein model ![](/assets/images/2016_04_icon4neworange.gif)
    -   Macro-mixing KB Go/DoME model ![](/assets/images/2016_04_icon4neworange.gif)
-   3SPN.2C DNA model ![](/assets/images/2016_04_icon4neworange.gif)
-   Structure-based RNA model ![](/assets/images/2016_04_icon4neworange.gif)
-   HPS/KH model for IDR and RNA ![](/assets/images/2016_04_icon4neworange.gif)
-   PWMcos model for protein-DNA sequence-specific interaction ![](/assets/images/2016_04_icon4neworange.gif)
-   A tool for preparing CG files: [GENESIS-CG-tool](https://github.com/genesis-release-r-ccs/genesis_cg_tool) ![](/assets/images/2016_04_icon4neworange.gif)
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
    -   GB/SA model with CHARMM and AMBER force fields ![](/assets/images/2016_04_icon4neworange.gif)
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
-   Free-energy perturbation method (FEP)** **
    -   Hybrid topology scheme
    -   Soft core potentials for non-bonded interactions
    -   Lambda-exchange (Hamiltonian exchange) FEP
    -   GPGPU acceleration
-   Computation with working precision (Single/Double precision)
-   GPGPU calculation in SPDYN
-   TIP4P water model in SPDYN
-   Trajectory analysis tools (Bond, Angle, Dihedral, RMSD, etc + 18
    tools)
-   Spatial decomposition analysis (SPANA)
-   Morphing calculation(ATDYN) ![](/assets/images/2016_04_icon4neworange.gif)

## Change log

-   Dec 10, 2021
    -   (SPDYN) Introduce AMBER FF19SB
    -   (ATDYN) Introduce AMBER GBSA model
    -   (SPDYN) Fix FEP
    -   (ATDYN) Fix GBSA
    -   (ATDYN) Fix CG model
    -   (ATDYN) Fix QM/MM
-   Nov 17, 2021:  Corrected the description of QM/MM and updated the
    manual.
-   Oct 6, 2021: GENESIS 1.7.0 released
    -   (ATDYN) AICG2+ CG model for protein
    -   (ATDYN) Macro-mixing KB Go/DoME model
    -   (ATDYN) 3SPN.2C CG model for DNA
    -   (ATDYN) Structure-based CG model for RNA
    -   (ATDYN) HPS/KH CG model for IDR and RNA
    -   (ATDYN) PWMcos CG model for protein-DNA sequence-specific
        interaction
    -   (ATDYN) Morphing calculation
    -   (Analysis) CG topology/parameter/coordinate generation:
        GENESIS-CG-tool
    -   (Analysis) Q-value analysis for CG models
    -   (Analysis) Extracting averages of selected atoms

