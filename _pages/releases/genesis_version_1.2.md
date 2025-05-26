---
permalink: /releases/genesis_version_1.2/
title: "GENESIS Version 1.2"
excerpt: "Release of GENESIS v1.2."
last_modified_at: 2025-05-13T11:59:26-09:00
toc: false
sidebar:
  title: "GENESIS Releases"
  nav: sidebar-releases
---

##  List of functions in ver. 1.2.1

-   MD, REMD, and Minimization
-   Computation with working precision (Single/Double precision)
-   GPGPU calculation in SPDYN
-   CHARMM C27, C36 force fields
-   AMBER force fields
-   MARTINI force fields
-   Go models (Cα, all-atom, and KB Go models)
-   Particle mesh Ewald method
-   Restraints (Position, distance, angle, dihedral, and PCA)
-   Constraints (SHAKE, SETTLE, RATTLE)
-   Leapfrog Verlet and Velocity Verlet integrators
-   RESPA integrator
-   NVE, NVT, NPT, NPAT, NPγT ensembles
-   Langevin thermostat and barostat
-   Berendsen thermostat and barostat
-   Bussi thermostat and barostat
-   Temperature, Pressure, Surface-tension REMD
-   Replica-exchange umbrella sampling (REUS or H-REMD)
-   Collective variable REUS
-   Multi-dimensional REMD
-   String method
-   String method on Cartesian space[\
    ]{style="text-decoration: underline;"}
-   Simulated annealing MD
-   Steered MD and Targeted MD
-   Trajectory analysis tools (Bond, Angle, Dihedral, RMSD)
-   TIP4P water model (AMBER/CHARMM; spdyn
    only) ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}

## Change log

-   Feb 7, 2018: GENESIS 1.2.1 released
    -   (spdyn) Fix constraints on minimization
    -   (atdyn) Fix dispersion correction for AMBER FF
    -   (atdyn/spdyn) Fix memory allocation bug in off-grid REUS (if \#
        of restraints in off-grid REUS \> \# of replicas)
    -   (general) extend maximum characters per line in control file
        (for recent CHARMM-GUI)
-   Jan 19, 2018: GENSIS 1.2.0 released
    -   source tree structure is modified
    -   (spdyn) TIP4P water model enabled (AMBER/CHARMM)
    -   (spdyn) minor bugfixes on domain specification
    -   (atdyn/spdyn) minor fixes for rpath parameters
    -   (build) support parallel compilation (e.g. make -j4)
    -   (build) enable_debug=4 enabled for gfortran

