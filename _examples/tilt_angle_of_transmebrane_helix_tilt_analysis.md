---
title: "GENESIS Input Example: Tilt angle of transmebrane helix (`tilt_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---

`tilt_analysis` is a tool to compute the angle between the principal axis of a
helix and the z-axis (the bilayer normal).

```toml
[INPUT]
psffile        = RALP_DPPC.psf
reffile        = RALP_DPPC.pdb
 
[OUTPUT]
txtfile        = tilt_output.txt   # output file name

[TRAJECTORY]
trjfile1       = RALP_DPPC_run.dcd
md_step1       = 10
mdout_period1  = 1
ana_period1    = 1
trj_format     = DCD
trj_type       = COOR+BOX
trj_natom      = 0

[SELECTION]
group1 = segid:PROA & an:CA & resno:4-28
 
[OPTION]
check_only     = NO
TM_helix_atom  = 1      # TM helix atoms to be analyzed
```
