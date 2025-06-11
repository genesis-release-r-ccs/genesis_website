---
title: "GENESIS Input Example: Membrane thickness (`lipidthick_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---

This tool calculates thickness of lipid bilayers, the distance in z-direction
between the phosphate groups in upper leaflet and ones in lower leaflet.

```toml
[INPUT]
psffile = DPPC.psf
reffile = DPPC.pdb

[OUTPUT]
outfile = DPPC_thickness.txt

[TRAJECTORY]
trjfile1      = DPPC_run.dcd
md_step1      = 10000
mdout_period1 = 10
ana_period1   = 10

trj_format    = DCD
trj_type      = COOR+BOX

[SELECTION]
group1        = rnam:DPPC & (an:P) # specify atoms used for the lipidthick measurement

[OPTION]
check_only    = NO
membrane_atom = 1     # atom group representing lipid bilayer
```
