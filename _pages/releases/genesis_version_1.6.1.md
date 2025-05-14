---
permalink: /releases/genesis_version_1.6.1/
title: "GENESIS Version 1.6.1"
excerpt: "Release of GENESIS v1.6.1."
last_modified_at: 2025-05-13T11:59:26-09:00
toc: false
sidebar:
  title: "GENESIS Releases"
  nav: sidebar-releases
---

### GENESIS version 1.6

#####  Download ver. 1.6.2 

+-----------------------+-----------------------+-----------------------+
| **Source code**       | Sep 27, 2021          | :::: sdm_downlo       |
|                       |                       | ad_button_box_default |
|                       |                       | ::: sdm_download_link |
|                       |                       | [Download             |
|                       |                       | (genesis-1.6.2        |
|                       |                       | .tar.bz2)](index.html |
|                       |                       | @sdm_process_download |
|                       |                       | =1&download_id=16710  |
|                       |                       | "GENESIS 1.6.2 Source |
|                       |                       |  Code"){.sdm_download |
|                       |                       | .grey}                |
|                       |                       | :::                   |
|                       |                       | ::::                  |
+-----------------------+-----------------------+-----------------------+
| **User manual\        | Sep 27, 2021          | :::: sdm_downlo       |
| **                    |                       | ad_button_box_default |
|                       |                       | ::: sdm_download_link |
|                       |                       | [Download             |
|                       |                       | (gene                 |
|                       |                       | sis-1.6.2.pdf)](index |
|                       |                       | .html@sdm_process_dow |
|                       |                       | nload=1&download_id=1 |
|                       |                       | 6712 "GENESIS 1.6.2 M |
|                       |                       | anual"){.sdm_download |
|                       |                       | .grey}                |
|                       |                       | :::                   |
|                       |                       | ::::                  |
+-----------------------+-----------------------+-----------------------+
| **Test set**          | Dec 10, 2021          | :::: sdm_downlo       |
|                       |                       | ad_button_box_default |
|                       |                       | ::: sdm_download_link |
|                       |                       | [Download             |
|                       |                       | (tes                  |
|                       |                       | ts-1.6.2_211210.tar.b |
|                       |                       | z2)](index.html@sdm_p |
|                       |                       | rocess_download=1&dow |
|                       |                       | nload_id=17222 "GENES |
|                       |                       | IS 1.6.2 Tests Dec 10 |
|                       |                       |  2021"){.sdm_download |
|                       |                       | .grey}                |
|                       |                       | :::                   |
|                       |                       | ::::                  |
+-----------------------+-----------------------+-----------------------+

The license of GENESIS source code is [the GNU Lesser General Public
License version 3](https://www.gnu.org/licenses/lgpl-3.0.en.html).

#####  List of functions in ver. 1.6.2 

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
    -   Minimum energy path search using String
        methods**[![](assets/images/2016_04_icon4neworange.gif){.alignnone
        .size-full .wp-image-911 decoding="async" width="36" height="15"
        srcset="wp-content/uploads/2016/04/icon4neworange.gif 36w, wp-content/uploads/2016/04/icon4neworange-20x8.gif 20w"
        sizes="(max-width: 36px) 100vw, 36px"}](wp-content/uploads/2016/04/icon4neworange.gif)**
    -   Supported QM software:
        [Gaussian](http://gaussian.com/){target="_blank"
        rel="noopener noreferrer"},
        [Q-Chem](http://www.q-chem.com){target="_blank"
        rel="noopener noreferrer"},
        [TeraChem](http://www.petachem.com){target="_blank"
        rel="noopener noreferrer"},
        [DFTB+](https://www.dftbplus.org){target="_blank"
        rel="noopener noreferrer"},
        [QSimulate-QM](https://qsimulate.com/academic.html){target="_blank"
        rel="noopener noreferrer"}**[![](assets/images/2016_04_icon4neworange.gif){.alignnone
        .size-full .wp-image-911 decoding="async" width="36" height="15"
        srcset="wp-content/uploads/2016/04/icon4neworange.gif 36w, wp-content/uploads/2016/04/icon4neworange-20x8.gif 20w"
        sizes="(max-width: 36px) 100vw, 36px"}](wp-content/uploads/2016/04/icon4neworange.gif)**
    -   Available MM force field: CHARMM and
        AMBER**[![](assets/images/2016_04_icon4neworange.gif){.alignnone
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

[Change log]{#id680b381734808 .collapseomatic tabindex="0"
title="Change log"}

-   Dec 23, 2020: GENESIS 1.6.0 released\
    -   (ATDYN) QM/MM MD
    -   (ATDYN) Minimum energy path search
    -   (TOOL) SPANA (sasa, rdf, density, hbond, contact)

```{=html}
<!-- -->
```
-   Aug 1, 2021: GENESIS 1.6.1 released\
    -   (ATDYN) QM/MM with AMBER force field
    -   (ATDYN) Interface with QSimulate-QM

```{=html}
<!-- -->
```
-   Sep 27, 2021: GENESIS 1.6.2 released\
    -   (SPDYN/ATDYN) Compiling and running GENESIS on Windows 10
    -   (TOOL) Deprecate energy_analysis

\
[Older versions]{#id680b381734845 .collapseomatic tabindex="0"
title="Older versions"}

##### Download ver. 1.6.1 

+-----------------------+-----------------------+-----------------------+
| **Source code**       | Aug 1, 2021           | :::: sdm_downlo       |
|                       |                       | ad_button_box_default |
|                       |                       | ::: sdm_download_link |
|                       |                       | [Download             |
|                       |                       | (genesis-1.6.1        |
|                       |                       | .tar.bz2)](index.html |
|                       |                       | @sdm_process_download |
|                       |                       | =1&download_id=16435  |
|                       |                       | "GENESIS 1.6.1 Source |
|                       |                       |  Code"){.sdm_download |
|                       |                       | .grey}                |
|                       |                       | :::                   |
|                       |                       | ::::                  |
+-----------------------+-----------------------+-----------------------+
| **User manual\        | Aug 1, 2021           | :::: sdm_downlo       |
| **                    |                       | ad_button_box_default |
|                       |                       | ::: sdm_download_link |
|                       |                       | [Download             |
|                       |                       | (gene                 |
|                       |                       | sis-1.6.1.pdf)](index |
|                       |                       | .html@sdm_process_dow |
|                       |                       | nload=1&download_id=1 |
|                       |                       | 6432 "GENESIS 1.6.1 M |
|                       |                       | anual"){.sdm_download |
|                       |                       | .grey}                |
|                       |                       | :::                   |
|                       |                       | ::::                  |
+-----------------------+-----------------------+-----------------------+
| **Test set**          | Dec 10, 2021          | :::: sdm_downlo       |
|                       |                       | ad_button_box_default |
|                       |                       | ::: sdm_download_link |
|                       |                       | [Download             |
|                       |                       | (tes                  |
|                       |                       | ts-1.6.1_211210.tar.b |
|                       |                       | z2)](index.html@sdm_p |
|                       |                       | rocess_download=1&dow |
|                       |                       | nload_id=17221 "GENES |
|                       |                       | IS 1.6.1 Tests Dec 10 |
|                       |                       |  2021"){.sdm_download |
|                       |                       | .grey}                |
|                       |                       | :::                   |
|                       |                       | ::::                  |
+-----------------------+-----------------------+-----------------------+

