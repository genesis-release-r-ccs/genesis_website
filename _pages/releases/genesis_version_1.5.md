---
permalink: /releases/genesis_version_1.5/
title: "GENESIS Version 1.5"
excerpt: "Release of GENESIS v1.5."
last_modified_at: 2025-05-13T11:59:26-09:00
toc: false
sidebar:
  title: "GENESIS Releases"
  nav: sidebar-releases
---

The license of GENESIS source code is changed from [the GNU General Public
License version 2](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html) to
[the GNU Lesser General Public License version
3](https://www.gnu.org/licenses/lgpl-3.0.en.html). The user manual for 1.5.1 is
currently being prepared. The usage of 1.5.1 is almost the same as 1.4.0 except
for the free-energy perturbation (FEP) method. If you use the FEP method, see
Tutorial 15.1.

##  List of functions in ver. 1.5.1

-   CHARMM and AMBER force fields
-   MARTINI model
-   Go models (Cα, all-atom, and KB Go models)
-   Energy minimization
    -   Steepest descent method
    -   LBFGS
-   Molecular dynamics simulation
    -   Leapfrog, velocity Verlet, and RESPA integrators
    -   NVE, NVT, NPT, NPAT, NPγT ensembles
    -   Langevin, Berendsen, and Bussi thermostat/barostat
    -   Particle mesh Ewald method
    -   GB/SA implicit solvent model
    -   Constraints (SHAKE, RATTLE, SETTLE, LINCS)
    -   Restraints (Position, distance, angle, dihedral, PCA)
    -   Simulated annealing MD
    -   Steered MD and Targeted MD
-   Replica-exchange molecular dynamics method
    -   Temperature, Pressure, Surface-tension REMD
    -   Replica-exchange umbrella sampling (REUS or H-REMD)
    -   Generalized replica exchange with solute tempering (gREST)
    -   On-the-fly energy analysis in gREST ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
    -   Multi-dimensional REMD (T-REMD/REUS, gREST/REUS)
    -   Collective variable REUS
-   Boundary
    -   Periodic boundary condition (PBC)
    -   Isolated system (no-PBC w/ or w/o spherical boundary potential) ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
-   String method
-   Gaussian accelerated molecular dynamics method (GaMD, GaREUS)
-   QM/MM calculation
-   Anharmonic vibrational analysis
    (+[SINDO](https://tms.riken.jp/en/research/software/sindo/))
-   Cryo-EM flexible fitting
-   Free-energy perturbation method (FEP) ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
    -   Hybrid topology scheme
    -   Soft core potentials for non-bonded interactions
    -   Lambda-exchange (Hamiltonian exchange) FEP
    -   GPGPU acceleration
-   Computation with working precision (Single/Double precision)
-   GPGPU calculation in SPDYN
-   TIP4P water model in SPDYN
-   Trajectory analysis tools (Bond, Angle, Dihedral, RMSD, etc + 18
    tools)

## Change log

-   Sep 3, 2020: GENESIS 1.5.1 released
    -   (ATDYN) Fix L-BFGS minimization
    -   (SPDYN) Fix free-energy perturbation
    -   (SPDYN) Enable local restraints in free-energy perturbation
    -   (Analysis) Fix mbar_analysis
    -   (Analysis) Stop energy_analysis if there is non-desired mixtures
        of solvent and solute in angle/dihed/improp/cmap
    -   (general) extend maximum characters per line in control file
        (for recent CHARMM-GUI)
-   May 1, 2020: GENESIS 1.5.0 released\
    -   (ATDYN) Add spherical boundary potential for no-PBC systems
    -   (ATDYN) Compile without using OpenMP
    -   (SPDYN) On-the-fly energy analysis in gREST
    -   (SPDYN) Free-energy perturbation
    -   (ATDYN, SPDYN) Add an option to avoid shrinkage in string
        methods
    -   (ATDYN, SPDYN) MRC/CCP4 format is available for the target
        cryo-EM density map file
