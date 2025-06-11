---
title: "GENESIS Input Example: Visualization of the PC vectors by VMD or PyMol (`pcavec_drawer`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---

This tool generates VMD and PyMol files to visualize PCA vectors. The
parameters in `[OPTION]` specify length, size, and color of vectors
drawn in VMD and PyMol.

```toml
[INPUT]
pdb_avefile = ../1_avecrd/output_ave.pdb   # PDB file obtained by avecrd_analysis
vecfile     = ../3_eigmat/output.vec       # VEC file obtained by eigmat_analysis
 
[OUTPUT]
vmdfile     = output.vmd                   # VMD visualization state file
pmlfile     = output.pml                   # PyMol script file
 
[OPTION]
check_only       = NO                      # (YES/NO)
mode_no          = 1                       # PCA mode number to be visualized
expand_vector    = 25.0
arrow_length     = 1.5
cylinder_radius  = 0.15
cone_radius      = 0.45
arrow_reverse    = NO                      # reverse the arrow direction
vector_color_vmd = yellow                  # color name for [VMD]
vector_color_pml = 0, 1, 1                 # RGB color (0.0-1.0) for [PyMOL]
```

For VMD, run the following command:
```bash
$ vmd ../1_avecrd/output_ave.pdb -e output.vmd
```

![](/assets/images/2017_05_vecvmd.png)

For PyMol, run the following command:
```bash
$ pymol output.pml

PyMOL> set ribbon_sampling, 1
PyMOL> set ribbon_trace, 1
PyMOL> set ribbon_width, 8
PyMOL> show ribbon, output_ave
```

![](/assets/images/2017_05_vecpml.png)

