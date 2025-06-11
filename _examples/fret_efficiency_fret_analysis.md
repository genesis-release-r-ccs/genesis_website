---
title: "GENESIS Input Example: FRET efficiency (`fret_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


`fret_analysis` is a tool to compute FRET efficiency between two dyes.

```toml
[INPUT]
psffile        = FRET_test_insulin.psf
reffile        = FRET_test_insulin.pdb
 
[OUTPUT]
outfile        = FRET_efficiency.out   # output file name

[TRAJECTORY]
trjfile1       = FRET_test_insulin.dcd
md_step1       = 10
mdout_period1  = 1
ana_period1    = 1
trj_format     = DCD
trj_type       = COOR+BOX
trj_natom      = 0

[SELECTION]
group1 = rnam:CY3R & (an:NZM | an:CMX | an:CMY          \                        or an:CM1 | an:CM2 | an:CM3 | an:CM4 \                        or an:CM5 | an:CM6 | an:CM7 | an:CM8 \                        or an:SM  | an:OM1 | an:OM2 | an:OM3 \                        or an:CD1 | an:CD2 | an:CD3          \                        or an:NZN | an:CNX | an:CNY          \                        or an:CN1 | an:CN2 | an:CN3 | an:CN4 \                        or an:CN5 | an:CN6 | an:CN7 | an:CN8 \                        or an:SN  | an:ON1 | an:ON2 | an:ON3)

group2 = rnam:CY5R & (an:NZM | an:CMX | an:CMY          \                           or an:CM1 | an:CM2 | an:CM3 | an:CM4 \                        or an:CM5 | an:CM6 | an:CM7 | an:CM8 \                        or an:SM  | an:OM1 | an:OM2 | an:OM3 \                        or an:CD1 | an:CD2 | an:CD3 | an:CD4 | an:CD5 \                        or an:NZN | an:CNX | an:CNY          \                        or an:CN1 | an:CN2 | an:CN3 | an:CN4 \                        or an:CN5 | an:CN6 | an:CN7 | an:CN8 \                        or an:SN  | an:ON1 | an:ON2 | an:ON3)
[OPTION]
check_only     = NO
analysis_atom_1 = 1
analysis_atom_2 = 2
Forster_radius  = 56.0
```
