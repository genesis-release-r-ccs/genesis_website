---
title: "GENESIS Input Example: Calculation of the averaged coordinates (`avecrd_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:37+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


This tool is also used for the principal component analysis (PCA). 
See [Tutorials 2.1](/tutorials/genesis_tutorial_2.1_2022/) and
[4.4](/tutorials/genesis_tutorial_4.4_2022/).

## Calculation of the averaged coordinates of the protein CA atoms

```toml
[INPUT]
reffile         = 2ech.pdb        # PDB file
psffile         = 2ech.psf        # PSF file
 
[OUTPUT]
pdbfile         = output.pdb      # Coordinates of reference structure
rmsfile         = output.rms      # RMSD with respect to the average
pdb_avefile     = output_ave.pdb  # Averaged coordinates of analysis atoms
pdb_aftfile     = output_aft.pdb  # Averaged coordinates of fitting atoms
 
[TRAJECTORY]
trjfile1        = md.dcd          # trajectory file
md_step1        = 5000            # number of MD steps
mdout_period1   = 1               # MD output period
ana_period1     = 1               # analysis period
repeat1         = 1
trj_format      = DCD             # (PDB/DCD)
trj_type        = COOR+BOX        # (COOR/COOR+BOX)
trj_natom       = 0               # (0:uses reference PDB atom count)
 
[SELECTION]
group1          = an:CA           # selection group 1
 
[FITTING]
fitting_method  = TR+ROT          # [NO,TR,TR+ROT,TR+ZROT,XYTR,XYTR+ZROT]
fitting_atom    = 1               # atom group
mass_weight     = NO              # mass-weight is not applied

[OPTION]
check_only      = NO              # (YES/NO)
num_iterations  = 10              # number of iterations
analysis_atom   = 1               # analysis target atom group
```
