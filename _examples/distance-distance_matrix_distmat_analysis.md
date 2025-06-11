---
title: "GENESIS Input Example: Distance-distance matrix (`distmat_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:37+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


This tool calculates the averaged distances between all pairs of the selected
atoms. The results of the distance are output in a matrix format.
```toml
[INPUT]
psffile        = BPTI_ionize.psf   # protein structure file
reffile        = BPTI_ionize.pdb   # PDB file

[OUTPUT]
outfile        = output.out        # output file

[TRAJECTORY]
trjfile1       = ../BPTI_run.dcd   # trajectory file
md_step1       = 500000            # number of MD steps
mdout_period1  = 500               # MD output period
ana_period1    = 500               # analysis period
repeat1        = 1
trj_format     = DCD               # (PDB/DCD)
trj_type       = COOR+BOX          # (COOR/COOR+BOX)
trj_natom      = 0                 # (0:uses reference PDB atom count)

[SELECTION]
group1         = an: CA            # selection group 1

[OPTION]
check_only     = NO                # (YES/NO)
analysis_atom  = 1                 # atom group
matrix_shape   = HALF              # (HALF/FULL)
```
