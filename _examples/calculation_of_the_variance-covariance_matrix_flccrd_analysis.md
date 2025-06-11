---
title: "GENESIS Input Example: Calculation of the variance-covariance matrix (`flccrd_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:37+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


`flccrd_anslysis` compute the RMSF (Root-mean-square fluctuation) of the
selected atoms. This tool is also used for the principal component
analysis (PCA). 
See [Tutorials 2.1](/tutorials/genesis_tutorial_2.1_2022/) and
[4.4](/tutorials/genesis_tutorial_4.4_2022/).
In the case of "`vcv_matrix = Global`" analysis_atoms are analyzed, while in the
case of "`vcv_matrix = Local`" fitting_atoms are analyzed.

## Calculation of the RMSF of the protein CA atoms

```toml
[INPUT]
reffile     = 2ech.pdb                   # PDB file
psffile     = 2ech.psf                   # PSF file
pdb_avefile = ../1_avecrd/output_ave.pdb # PDB file obtained by avecrd_analysis
pdb_aftfile = ../1_avecrd/output_aft.pdb # PDB file obtained by avecrd_analysis

[OUTPUT]
pcafile         = output.pca      # PCA file
rmsfile         = output.rms      # RMSF file
vcvfile         = output.vcv      # Variance-Covarience Matrix file
crsfile         = output.crs      # CRS file

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
vcv_matrix      = Global          # (GLOBAL/LOCAL)
analysis_atom   = 1               # analysis target atom group
```
