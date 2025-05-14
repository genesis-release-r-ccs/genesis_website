---
title: "GENESIS Functions"
excerpt: "Supported Features of GENESIS."
last_modified_at: 2025-05-08T11:59:26+09:00
layout: single
toc: false
---

## Functions of GENESIS

The GENESIS package contains two MD programs (`atdyn` and `spdyn`), trajectory
analysis programs, and other useful tools.

-   CHARMM force field, AMBER force field, MARTINI model, Go models (Cα
    Go, all-atom Go, KB Go, and AICG2+), and residue-level
    coarse-grained DNA/RNA models
-   Energy minimization and molecular dynamics simulations
-   Leapfrog, velocity Verlet, and RESPA integrators
-   SHAKE/RATTLE, SETTLE, and LINCS algorithms for bond constraint
-   Bussi, Langevin, and Berendsen thermostat/barostat in the NVT, NPT,
    NPAT, and NPγT ensembles
-   Replica-exchange molecular dynamics method (REMD) in temperature,
    pressure, and surface-tension space
-   Generalized replica-exchange with solute tempering (gREST)
-   Replica-exchange umbrella sampling (REUS) with collective variables
-   Multi-dimensional REMD method (T-REMD/P-REMD, T-REMD/REUS,
    gREST/REUS, etc)
-   Gaussian accelerated molecular dynamics method (GaMD, GaREUS)
-   String method for reaction pathway search
-   Hybrid QM/MM (quantum mechanics/molecular mechanics) calculation
-   Implicit solvent model (Generalized Born/Solvent Accessible Surface
    Area model)
-   Cryo-EM flexible fitting using all-atom, CG, and implicit solvent
    models with MD, REMD, and REUS
-   Free-energy perturbation method (FEP)
-   Anharmonic vibrational analysis using SINDO
-   Steered MD and Targeted MD simulations
-   Restrained MD simulations (Distance, angle, dihedral angle,
    position, etc)
-   Hybrid MPI+OpenMP, hybrid CPU+GPGPU, mixed double+single precision
    calculations
-   Highly-parallelized Particle mesh Ewald method (PME)
-   Scalable MD simulations for huge systems (\> 100,000,000 atoms)
-   Trajectory analysis tools (RMSD, RMSF, PCA, WHAM, MBAR, etc.)
-   Spatial decomposition analysis (SPANA)

MD trajectories obtained by GENESIS can be visualized with
[VMD](http://www.ks.uiuc.edu/Research/vmd/) and other software.
[CHARMM-GUI](http://www.charmm-gui.org/) input generator can be used for the
preparation of GENESIS input files.
