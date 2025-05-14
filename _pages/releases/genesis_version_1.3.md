---
permalink: /releases/genesis_version_1.3/
title: "GENESIS Version 1.3"
excerpt: "Release of GENESIS v1.3."
last_modified_at: 2025-05-13T11.39:26-09:00
toc: false
sidebar:
  title: "GENESIS Releases"
  nav: sidebar-releases
---

### [GENESIS version 1.3]{style="color: #000000;"}

#####  Download ver. 1.3.0 

+-----------------------+-----------------------+-----------------------+
| **Source code**       | Jun 18, 2018          | :::: sdm_downlo       |
|                       |                       | ad_button_box_default |
|                       |                       | ::: sdm_download_link |
|                       |                       | [Download             |
|                       |                       | (genesis-1.3.         |
|                       |                       | 0.tar.bz2)](index.htm |
|                       |                       | l@sdm_process_downloa |
|                       |                       | d=1&download_id=6587  |
|                       |                       | "GENESIS 1.3.0 Source |
|                       |                       |  Code"){.sdm_download |
|                       |                       | .grey}                |
|                       |                       | :::                   |
|                       |                       | ::::                  |
+-----------------------+-----------------------+-----------------------+
| **User manual\        | Jun 18, 2018          | :::: sdm_downlo       |
| **                    |                       | ad_button_box_default |
|                       |                       | ::: sdm_download_link |
|                       |                       | [Download             |
|                       |                       | (GEN                  |
|                       |                       | ESIS-1.3.0.pdf)](inde |
|                       |                       | x.html@sdm_process_do |
|                       |                       | wnload=1&download_id= |
|                       |                       | 6585 "GENESIS 1.3.0 M |
|                       |                       | anual"){.sdm_download |
|                       |                       | .grey}                |
|                       |                       | :::                   |
|                       |                       | ::::                  |
+-----------------------+-----------------------+-----------------------+
| **Test set**          | Jun 18, 2018          | :::: sdm_downlo       |
|                       |                       | ad_button_box_default |
|                       |                       | ::: sdm_download_link |
|                       |                       | [Download             |
|                       |                       | (test                 |
|                       |                       | s-1.3.0.tar.bz2)](ind |
|                       |                       | ex.html@sdm_process_d |
|                       |                       | ownload=1&download_id |
|                       |                       | =6588 "GENESIS 1.3.0  |
|                       |                       | Tests"){.sdm_download |
|                       |                       | .grey}                |
|                       |                       | :::                   |
|                       |                       | ::::                  |
+-----------------------+-----------------------+-----------------------+

#####  List of functions in ver. 1.3.0  {#list-of-functions-in-ver.-1.3.0 style="text-align: left;"}

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
-   TIP4P water model (AMBER/CHARMM; spdyn only)
-   Generalized replica exchange with solute tempering
    (gREST) **[![](assets/images/2016_04_icon4neworange.gif){.alignnone
    .size-full .wp-image-911 decoding="async" height="15" width="36"
    srcset="wp-content/uploads/2016/04/icon4neworange.gif 36w, wp-content/uploads/2016/04/icon4neworange-20x8.gif 20w"
    sizes="(max-width: 36px) 100vw, 36px"}](wp-content/uploads/2016/04/icon4neworange.gif)**

[Change log]{#id680b380c07ca8 .collapseomatic tabindex="0"
title="Change log"}

-   Jun 18, 2018: GENESIS 1.3.0 released
    -   (spdyn/atdyn) Enable generalized replica exchange with solute
        tempering (gREST)
    -   (spdyn) Improvement of nonbonded energy in GPU usage
    -   (spdyn/atdyn) Enable options,
        pressure_virial/pressure_rmsd,including virial terms from
        position/rmsd restraints in pressure evaluations
    -   (spdyn) Fix setup of AMBER FF when constraint is not set
    -   (atdyn) Fix PC restraints for computational performance

