---
title: "GENESIS Input Example: Projection of the coordinates trajectory onto PC axes (`prjcrd_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---

See [Tutorials 2.1](/tutorials/genesis_tutorial_2.1_2022/) and
[4.4](/tutorials/genesis_tutorial_4.4_2022/).


## Projection of each snapshot of MD trajectories onto PC axes

```toml
[INPUT]
reffile     = 2ech.pdb                    # PDB file
psffile     = 2ech.psf                    # PSF file
pdb_avefile = ../1_avecrd/output_ave.pdb  # PDB file obtained by avecrd_analysis
pdb_aftfile = ../1_avecrd/output_aft.pdb  # PDB file obtained by avecrd_analysis
valfile     = ../3_eigmat/output.val      # VAL file obtained by eigmat_analysis
vecfile     = ../3_eigmat/output.vec      # VEC file obtained by eigmat_analysis
 
[OUTPUT]
prjfile     = output.prj                  # PRJ file

[TRAJECTORY]
trjfile1        = md2.dcd        # trajectory file
md_step1        = 5000           # number of MD steps
mdout_period1   = 1              # MD output period
ana_period1     = 1              # analysis period
repeat1         = 1
trj_format      = DCD            # (PDB/DCD)
trj_type        = COOR+BOX       # (COOR/COOR+BOX)
trj_natom       = 0              # (0:uses reference PDB atom count)
 
[SELECTION]
group1          = an:CA          # selection group 1 
 
[FITTING]
fitting_method  = TR+ROT         # [NO,TR,TR+ROT,TR+ZROT,XYTR,XYTR+ZROT]
fitting_atom    = 1              # atom group
mass_weight     = NO             # mass-weight is not applied
 
[OPTION]
check_only      = NO             # (YES/NO)
vcv_matrix      = Global         # (GLOBAL/LOCAL)
num_pca         = 2              # # of principal components
analysis_atom   = 1              # analysis target atom group
```
