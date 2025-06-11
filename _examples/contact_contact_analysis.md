---
title: "GENESIS Input Example: Contact (`contact_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:37+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


`contact_analysis` calculates number of contact (atom pairs whose distance is
smaller than the `[range]`)  between the groups specified in the section
`[SELECTION]`. Image atoms around the unit box are also included in the analysis
if the boundary type is PBC.

![](/assets/images/2017_04_contact.bmp)

## (EX1) Multiple villins under the crowding condition

In this example, number of contacts between multiple (22) villins in crowding
condition are calculated. The matrix of the contact numbers between the `group_ij`
are output when `mode=number` is specified. 

```toml
[INPUT]
psffile = villin_crowding.psf
reffile = villin_crowding.pdb

[OUTPUT]
txtfile = villin_number.out

[TRAJECTORY]
trjfile1      = prod.dcd 
md_step1      = 10000 
mdout_period1 = 500 
ana_period1   = 500 
repeat1       = 1
trj_format    = DCD 
trj_type      = COOR+BOX 

[BOUNDARY]
type          = PBC
box_size_x    = 142 
box_size_y    = 142
box_size_z    = 142 
domain_x      = 2 
domain_y      = 2 
domain_z      = 2 
num_cells_x   = 14
num_cells_y   = 14
num_cells_z   = 14

[ENSEMBLE]
ensemble = NPT

[SELECTION]
group1 = ai: 1 - 596
group2 = ai: 597 - 1192
group3 = ai: 1193 - 1788
group4 = ai: 1789 - 2384
group5 = ai: 2385 - 2980
group6 = ai: 2981 - 3576
group7 = ai: 3577 - 4172
group8 = ai: 4173 - 4768
group9 = ai: 4769 - 5364
group10 = ai: 5365 - 5960
group11 = ai: 5961 - 6556
group12 = ai: 6557 - 7152
group13 = ai: 7153 - 7748
group14 = ai: 7749 - 8344
group15 = ai: 8345 - 8940
group16 = ai: 8941 - 9536
group17 = ai: 9537 - 10132
group18 = ai: 10133 - 10728
group19 = ai: 10729 - 11324
group20 = ai: 11325 - 11920
group21 = ai: 11921 - 12516
group22 = ai: 12517 - 13112

[SPANA_OPTION]
buffer    = 10
wrap      = yes 
box_size  = TRAJECTORY

[CONTACT_OPTION]
range = 10
mode  = number
```

`mode=mindist` outputs atomic pairs which gives the minimum distance between
`group_ij`.

```toml
[CONTACT_OPTION]
mode = mindist
```
