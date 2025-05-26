---
permalink: /releases/genesis_version_1.1/
title: "GENESIS Version 1.1"
excerpt: "Release of GENESIS v1.1."
last_modified_at: 2025-05-13T11:59:26-09:00
toc: false
sidebar:
  title: "GENESIS Releases"
  nav: sidebar-releases
---

##  List of functions in ver. 1.1.6 

-   MD, REMD, and Minimization
-   Computation with working precision (Single/Double precision) ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
-   GPGPU calculation in SPDYN ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
-   CHARMM C27, C36 force fields
-   AMBER force fields ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
-   MARTINI force fields ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
-   Go models (Cα, all-atom, and KB Go models)
-   Particle mesh Ewald method
-   Restraints (Position, distance, angle, dihedral, and PCA ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
-   Constraints (SHAKE, SETTLE, RATTLE)
-   Leapfrog Verlet and Velocity Verlet integrators
-   RESPA integrator ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
-   NVE, NVT, NPT, NPAT, NPγT ensembles
-   Langevin thermostat and barostat
-   Berendsen thermostat and barostat
-   Bussi thermostat and barostat ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
-   Temperature, Pressure, Surface-tension REMD
-   Replica-exchange umbrella sampling (REUS or H-REMD)
-   Collective variable REUS ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
-   Multi-dimensional REMD
-   String method ![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
-   String method on Cartesian space (1.1.2 or later)![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
-   Simulated annealing MD
-   Steered MD and Targeted MD![](/assets/images/2016_04_icon4neworange.gif){: width="40"}
-   Trajectory analysis tools (Bond, Angle, Dihedral, RMSD)

# Change log

-   Oct 27, 2017: minor update of manual
-   Oct 17, 2017: GENESIS 1.1.6 released
    -   (spdyn) fix of GPU code for newer NVIDIA drivers (version \>
        380)
    -   (configuration) fix of configure files
    -   (atdyn/spdyn) enable restarting minimization
    -   (atdyn) nonb_limiter is enabled
-   Jul 10, 2017: GENESIS 1.1.5 released
    -   (atdyn/spdyn) Fix of R-path (string method)
    -   (atdyn/spdyn) Fitting method (used in targeted MD, steered MD,
        string method etc.) renewal; \[FITTING\] section is introduced
        to extend the capability of structure fitting.
    -   (manual) description concerning \[FITTING\] was added
    -   (spdyn) improve GPU detection mechanism (automatically ignore
        GPUs with CC \< 3.5)
    -   (atdyn/spdyn) fix random generator used in BUSSI
        thermostat/barostat
    -   (spdyn) fix for communication tag
    -   (spdyn/tests) update of BUSSI NVT (regression test is also
        updated)
    -   (atdyn/spdyn) fix of gromacs input for SMOG2
    -   (atdyn/spdyn) fix of restart file reader (K-computer and FX100
        only; there could be large delay when reading input files (not
        always))
    -   (tests) test.py script now accepts relative path for the
        atdyn/spdyn executables
-   Jul 10, 2017: GENESIS 1.1.1-1.1.3 source codes were removed due to a
    potential license issue. (if you need them, please ask us via
    email.)
-   Jul 10, 2017: Source codes of 1.1.0 and 1.1.4 were updated to avoid
    a potential license issue
-   Mar 1, 2017: GENESIS 1.1.4 released
    -   (spdyn) additional fix for OpenMP (1.1.2, 1.1.3 users should
        update to this version)
-   Feb 24, 2017: GENESIS 1.1.3 released
    -   (spdyn) fix bug of contact_check when OpenMP thread number \> 1
        (not always)
    -   unnecessary files and directories are removed (Feb 27, 2017; the
        removed files might be unharmful)
-   Feb 22, 2017: GENESIS 1.1.2 released
    -   (spdyn) Cartesian coordinates can be employed as CV in the
        string method
    -   (rpath_generator) improved functionality
    -   (atdyn/spdyn) more smart handling of restart files
    -   (spdyn) show GPU hardware information in log file
    -   (spdyn) multiple GPUs in a single node can be used effectively
        in non-OpenMPI environment (such as Intel MPI)
    -   (spdyn) structure_check is implemented in spdyn (runtime
        structure check upon pairlist generation)
    -   (atdyn) support CHARMM19
-   Sep 07, 2016: GENESIS 1.1.1 released
    -   (code) fix problems while installing with CUDA-8.0RC and single
        precision
    -   (code) fix file I/O of the analysis tools (mbar_analysis,
        wham_analysis, pmf_analysis)
    -   (code) change default of pme_alpha in the \[ENERGY\] section
    -   (code) remove unnecessary error messages
    -   (tests) set pme_alpha in the control files due to change of the
        default
-   Aug 23, 2016: (manual) fix sample of the \[SELECTION\] section
    (p.43)
-   Jul 29, 2016: GENESIS 1.1.0 released

# Older versions

 Source codes of GENESIS 1.1.1, 1.1.2, 1.1.3 were removed to avoid potential
 license issue. If you need source code of those versions, please ask us via
 e-mail or something. GENESIS 1.1.0 and 1.1.4 were also updated for the same
 reason. This update does not cause any changes in their functions and results
 of calculations. (Jul.  10, 2017)
 
