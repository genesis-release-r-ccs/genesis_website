---
title: "GENESIS Input Example: Create synthetic EM density map from PDB file (`emmap_generator`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:37+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


This tool can generate a synthetic density map from a PDB file in a SITUS map
format. The map resolution is specified by` sigma` value in `[OPTION]`.  The
variable` auto_margin` makes a margin between in the map edge and
minimum/maximum coordinates of the input PDB file according to the specified
`margin_size_[x, y, z]` (For details, see the usage by `emmap_generator -h
ctrl`).


```toml
[INPUT]
pdbfile       = input.pdb     # PDB file

[OUTPUT]
mapfile       = output.sit    # SITUS file

[OPTION]
check_only    = NO
map_format    = SITUS         # (SITUS)
auto_margin   = YES           # (YES/NO)
margin_size_x = 20.0
margin_size_y = 20.0
margin_size_z = 20.0
voxel_size    = 1.0
sigma         = 2.5           # (half of map resolution)
tolerance     = 0.001
```
