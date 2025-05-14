---
permalink: /releases/genesis_version_1.6/
title: "GENESIS Version 1.6"
excerpt: "Release of GENESIS v1.6."
last_modified_at: 2025-05-13T11:59:26-09:00
toc: false
sidebar:
  title: "GENESIS Releases"
  nav: sidebar-releases
---

### GENESIS version 1.6

#####  Download ver. 1.6.0 

+-----------------------+-----------------------+-----------------------+
| **Source code**       | Dec 23, 2020          | :::: sdm_downlo       |
|                       |                       | ad_button_box_default |
|                       |                       | ::: sdm_download_link |
|                       |                       | [Download             |
|                       |                       | (genesis-1.6.0        |
|                       |                       | .tar.bz2)](index.html |
|                       |                       | @sdm_process_download |
|                       |                       | =1&download_id=15613  |
|                       |                       | "GENESIS 1.6.0 Source |
|                       |                       |  Code"){.sdm_download |
|                       |                       | .grey}                |
|                       |                       | :::                   |
|                       |                       | ::::                  |
+-----------------------+-----------------------+-----------------------+
| **User manual\        | Dec 23, 2020          | :::: sdm_downlo       |
| **                    |                       | ad_button_box_default |
|                       |                       | ::: sdm_download_link |
|                       |                       | [Download             |
|                       |                       | (gene                 |
|                       |                       | sis-1.6.0.pdf)](index |
|                       |                       | .html@sdm_process_dow |
|                       |                       | nload=1&download_id=1 |
|                       |                       | 5622 "GENESIS 1.6.0 M |
|                       |                       | anual"){.sdm_download |
|                       |                       | .grey}                |
|                       |                       | :::                   |
|                       |                       | ::::                  |
+-----------------------+-----------------------+-----------------------+
| **Test set**          | Dec 23, 2020          | :::: sdm_downlo       |
|                       |                       | ad_button_box_default |
|                       |                       | ::: sdm_download_link |
|                       |                       | [Download             |
|                       |                       | (tests                |
|                       |                       | -1.6.0.tar.bz2)](inde |
|                       |                       | x.html@sdm_process_do |
|                       |                       | wnload=1&download_id= |
|                       |                       | 15621 "GENESIS 1.6.0  |
|                       |                       | Tests"){.sdm_download |
|                       |                       | .grey}                |
|                       |                       | :::                   |
|                       |                       | ::::                  |
+-----------------------+-----------------------+-----------------------+

The license of GENESIS source code is changed from [the GNU General
Public License version
2](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html) to [the
GNU Lesser General Public License version
3](https://www.gnu.org/licenses/lgpl-3.0.en.html).

#####  List of functions in ver. 1.6.0 

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
        (+[SINDO](https://tms.riken.jp/en/research/software/sindo/){target="_blank"
        rel="noopener noreferrer"})
    -   QM/MM molecular
        dynamics**[![](assets/images/2016_04_icon4neworange.gif){.alignnone
        .size-full .wp-image-911 decoding="async" width="36" height="15"
        srcset="wp-content/uploads/2016/04/icon4neworange.gif 36w, wp-content/uploads/2016/04/icon4neworange-20x8.gif 20w"
        sizes="(max-width: 36px) 100vw, 36px"}](wp-content/uploads/2016/04/icon4neworange.gif)**
    -   Minimum energy path search using String/NEB
        methods**[![](assets/images/2016_04_icon4neworange.gif){.alignnone
        .size-full .wp-image-911 decoding="async" width="36" height="15"
        srcset="wp-content/uploads/2016/04/icon4neworange.gif 36w, wp-content/uploads/2016/04/icon4neworange-20x8.gif 20w"
        sizes="(max-width: 36px) 100vw, 36px"}](wp-content/uploads/2016/04/icon4neworange.gif)**
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
-   Spatial decomposition analysis
    (SPANA)**[![](assets/images/2016_04_icon4neworange.gif){.alignnone
    .size-full .wp-image-911 decoding="async" width="36" height="15"
    srcset="wp-content/uploads/2016/04/icon4neworange.gif 36w, wp-content/uploads/2016/04/icon4neworange-20x8.gif 20w"
    sizes="(max-width: 36px) 100vw, 36px"}](wp-content/uploads/2016/04/icon4neworange.gif)**

[Change log]{#id680b3813e1d1d .collapseomatic tabindex="0"
title="Change log"}

-   Dec 23, 2020: GENESIS 1.6.0 released\
    -   (ATDYN) QM/MM MD
    -   (ATDYN) Minimum energy path search
    -   (TOOL) SPANA (sasa, rdf, density, hbond, contact)

