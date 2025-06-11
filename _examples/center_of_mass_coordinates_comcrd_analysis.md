---
title: "GENESIS Input Example: Center of mass coordinates (`comcrd_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:37+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---

This tool analyzes the coordinates of the center of mass of the selected
atoms or molecules.

```toml
# Center of mass of the selected group
analysis_for = ALL

# Center of mass of each molecule in the selected group
analysis_for = MOLECULE
```

## XY-coordinates of the center of mass of lipid molecules around membrane proteins 

The output file contains the trajectory of the xy-coordinates of each
lipid molecules. The users can visualize lateral movement of the lipid*
molecules, and analyze the lateral diffusion of the molecules around the
membrane protein.

```toml
[INPUT]
psffile         = ../RALP_DPPC.psf
reffile         = ../RALP_DPPC_run.pdb
 
[OUTPUT]
trjfile         = output.trj              # trajectory file
 
[TRAJECTORY]
trjfile1        = ../RALP_DPPC_run.dcd    # trajectory file
md_step1        = 1000          # number of MD steps
mdout_period1   = 1             # MD output period
ana_period1     = 1             # analysis period
repeat1         = 1
trj_format      = DCD           # (PDB/DCD)
trj_type        = COOR+BOX      # (COOR/COOR+BOX)
trj_natom       = 0             # (0:uses reference PDB atom count)
 
[SELECTION]
group1          = segid:PROA    # membrane protein
group2          = rnam:DPPC     # lipid
 
[FITTING]
fitting_method  = TR            # fitting the membrane protein by translation
fitting_atom    = 1             # membrane protein
mass_weight     = NO            # mass-weight is not applied
 
[OPTION]
check_only      = NO
centering       = NO
analysis_atom   = 2             # select lipid 
analysis_for    = MOLECULE      # center of mass of molecules is analyzed
output_coord    = XY            # XY-coordinates of the center of mass
pbc_correct     = NO            # no wrap molecule
```
