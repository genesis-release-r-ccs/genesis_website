---
title: "GENESIS Input Example: Convert GENESIS restart file to PDB file (`rst_convert`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:37+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


This tool can convert a restart file in the GENESIS format (`rstfile`) to the
PDB file. If the user wants to wrap molecules into the unit cell,
specify `pbc_correct = MOLECULE` in `[OPTION]`. The restart files that were
generated from REMD simulations can be also treated by using `{}` in the file
name (For details, see the usage by `rst_convert -h ctrl`).


```toml
[INPUT]
rstfile       = input.rst     # GENESIS restart file
pdbfile       = input.pdb     # PDB file
psffile       = input.psf     # protein structure file

[OUTPUT]
pdbfile       = output.pdb    # PDB file

[OPTION]
check_only    = NO
rstin_format  = GENESIS
rstout_format = PDB
rstout_type   = AUTO
pbc_correct   = NO            # (NO/MOLECULE)
shift_coord   = 0.0 0.0 0.0   # displacement of translation
```
