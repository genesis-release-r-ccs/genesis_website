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

### [GENESIS version 1.2]{style="color: #000000;"}

#####  Download ver. 1.2.1 

+-----------------------+-----------------------+-----------------------+
| **Source code\        | Feb 7, 2018           | :::: sdm_downlo       |
| **                    |                       | ad_button_box_default |
|                       |                       | ::: sdm_download_link |
|                       |                       | [Download             |
|                       |                       | (genesis-1.2.         |
|                       |                       | 1.tar.bz2)](index.htm |
|                       |                       | l@sdm_process_downloa |
|                       |                       | d=1&download_id=6461  |
|                       |                       | "GENESIS 1.2.1 Source |
|                       |                       |  Code"){.sdm_download |
|                       |                       | .grey}                |
|                       |                       | :::                   |
|                       |                       | ::::                  |
+-----------------------+-----------------------+-----------------------+
| **User manual\        | Feb 7, 2018           | :::: sdm_downlo       |
| **                    |                       | ad_button_box_default |
|                       |                       | ::: sdm_download_link |
|                       |                       | [Download             |
|                       |                       | (GEN                  |
|                       |                       | ESIS-1.2.1.pdf)](inde |
|                       |                       | x.html@sdm_process_do |
|                       |                       | wnload=1&download_id= |
|                       |                       | 6459 "GENESIS 1.2.1 M |
|                       |                       | anual"){.sdm_download |
|                       |                       | .grey}                |
|                       |                       | :::                   |
|                       |                       | ::::                  |
+-----------------------+-----------------------+-----------------------+
| **Test set**          | Feb 7, 2018           | :::: sdm_downlo       |
|                       |                       | ad_button_box_default |
|                       |                       | ::: sdm_download_link |
|                       |                       | [Download             |
|                       |                       | (test                 |
|                       |                       | s-1.2.1.tar.bz2)](ind |
|                       |                       | ex.html@sdm_process_d |
|                       |                       | ownload=1&download_id |
|                       |                       | =6442 "GENESIS 1.2.1  |
|                       |                       | Tests"){.sdm_download |
|                       |                       | .grey}                |
|                       |                       | :::                   |
|                       |                       | ::::                  |
+-----------------------+-----------------------+-----------------------+

#####  List of functions in ver. 1.2.1  {#list-of-functions-in-ver.-1.2.1 style="text-align: left;"}

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
    only)[![](assets/images/2016_04_icon4neworange.gif){.alignnone
    .size-full .wp-image-911 decoding="async" height="15" width="36"
    srcset="wp-content/uploads/2016/04/icon4neworange.gif 36w, wp-content/uploads/2016/04/icon4neworange-20x8.gif 20w"
    sizes="(max-width: 36px) 100vw, 36px"}](wp-content/uploads/2016/04/icon4neworange.gif)

[Change log]{#id680b380b64b70 .collapseomatic tabindex="0"
title="Change log"}

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

[Older versions]{#id680b380b64bad .collapseomatic tabindex="0"
title="Older versions"}

+-----------------------+-----------------------+-----------------------+
| **Source code\        | Jan 19, 2018          | :::: sdm_downlo       |
| **                    |                       | ad_button_box_default |
|                       |                       | ::: sdm_download_link |
|                       |                       | [Download             |
|                       |                       | (genesis-1.2.         |
|                       |                       | 0.tar.bz2)](index.htm |
|                       |                       | l@sdm_process_downloa |
|                       |                       | d=1&download_id=6444  |
|                       |                       | "GENESIS 1.2.0 source |
|                       |                       |  code"){.sdm_download |
|                       |                       | .grey}                |
|                       |                       | :::                   |
|                       |                       | ::::                  |
+-----------------------+-----------------------+-----------------------+
| **User manual\        | Jan 19, 2018          | :::: sdm_downlo       |
| **                    |                       | ad_button_box_default |
|                       |                       | ::: sdm_download_link |
|                       |                       | [Download             |
|                       |                       | (GEN                  |
|                       |                       | ESIS-1.2.0.pdf)](inde |
|                       |                       | x.html@sdm_process_do |
|                       |                       | wnload=1&download_id= |
|                       |                       | 6408 "GENESIS 1.2.0 M |
|                       |                       | anual"){.sdm_download |
|                       |                       | .grey}                |
|                       |                       | :::                   |
|                       |                       | ::::                  |
+-----------------------+-----------------------+-----------------------+

\
\

