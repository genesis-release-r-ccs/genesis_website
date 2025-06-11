---
title: "GENESIS Input Example: Distance RMSD (`drms_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:37+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---

`drms_analysis` is a tool calculating distance RMS (dRMS) from
structure(s).

```toml
# example dRMS of inter domains (group1 and group3)
[INPUT]
psffile = psf
pdbfile = open.pdb
reffile = close.pdb

[OUTPUT]
rmsfile = drms.log # log file

[TRAJECTORY]
trjfile1 = pr.1.pdb # trajectory file
md_step1 = 1        # number of MD steps
mdout_period1 = 1   # MD output period
ana_period1 = 1     # analysis period
repeat1 = 1
trj_format = PDB    # (PDB/DCD)
trj_type = COOR     # (COOR/COOR+BOX)
trj_natom = 0       # (0:uses reference PDB atom count)

[SELECTION]
group1 = (resno:1-29) or (resno:68-117) or (resno:161-214)
group2 = (resno:30-67)
group3 = (resno:118-167)

[OPTION]
check_only = NO      # (YES/NO)
contact_groups = 1 3 #main-NMP
minimum_distance = 6.0
maximum_distance = 10.0
avoid_bonding = yes
exclude_residues = 0
```
