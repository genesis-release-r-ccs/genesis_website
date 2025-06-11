---
title: "GENESIS Input Example: Radius of gyration (`rg_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


# Calculation of the radius of gyration of the protein (segid:BPTI)

```toml
[INPUT]
psffile       = ../BPTI_ionize.psf # protein structure file
reffile       = ../BPTI_ionize.pdb # PDB file

[OUTPUT]
rgfile        = output.rg          # RG file

[TRAJECTORY]
trjfile1      = ../BPTI_run.dcd    # trajectory file
md_step1      = 500000             # number of MD steps
mdout_period1 = 500                # MD output period
ana_period1   = 500                # analysis period
repeat1       = 1
trj_format    = DCD                # (PDB/DCD)
trj_type      = COOR+BOX           # (COOR/COOR+BOX)
trj_natom     = 0                  # (0:uses reference PDB atom count)

[SELECTION]
group1        = segid:BPTI         # selection group 1

[OPTION]
check_only    = NO                 # (YES/NO)
analysis_atom = 1                  # atom group
mass_weighted = YES                # (YES/NO)
```
