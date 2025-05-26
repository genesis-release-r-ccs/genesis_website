---
permalink: /releases/genesis_version_2.0_beta/
title: "GENESIS Version 2.0beta"
excerpt: "Release of GENESIS v2.0β"
last_modified_at: 2025-05-13T11:59:26-09:00
toc: false
sidebar:
  title: "GENESIS Releases"
  nav: sidebar-releases
---

##  Download ver. 2.0 beta

The source code and user manual are available from [GitHub](https://github.com/genesis-release-r-ccs/genesis-2.0). Click the “`Code`” bottom and select “Download ZIP” in the page.

##  List of functions in ver. 2.0 beta 

Only SPDYN is available currently.  For the other functions that are not implemented in ver. 2.0 beta, please use ver. 1.7.

- CHARMM and AMBER force fields
- Energy minimization
  - Steepest descent method
- Molecular dynamics simulation
  - Velocity Verlet, and RESPA integrators
  - NVE, NVT, NPT ensembles
  - Bussi thermostat/barostat
  - Particle mesh Ewald method
  - Constraints (SHAKE, RATTLE, SETTLE)
  - Restraints (Position, distance, angle, dihedral, PCA)
  - Simulated annealing MD
  - Steered MD and Targeted MD
- Replica-exchange molecular dynamics method
  - Temperature, Pressure, Surface-tension REMD
  - Replica-exchange umbrella sampling (REUS or H-REMD)
  - Generalized replica exchange with solute tempering (gREST)
  - On-the-fly energy analysis in gREST ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
  - Multi-dimensional REMD (T-REMD/REUS, gREST/REUS)
  - Collective variable REUS
- Boundary
  -   Periodic boundary condition (PBC)
- String method
- Gaussian accelerated molecular dynamics method (GaMD, GaREUS)
- Computation with working precision (Single/Double precision)
- GPGPU calculation in SPDYN
- TIP4P water model in SPDYN

