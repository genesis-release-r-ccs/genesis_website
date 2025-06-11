---
title: "GENESIS Input Example: RMSD: root-mean-square deviation (`rmsd_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


In usual, we analyze RMSD of the protein Calpha atoms with respect to the
initial structure as in Ex1. `reffile` in `[INPUT]` is the reference
coordinates. We can also specify fitting atoms and analysis atoms separately.
For example, RMSD of the ligand with respect to the protein can be analyzed (see
Ex2).

## Ex1: RMSD with respect to the protein Calpha atoms in the initial structure

```toml
[INPUT]
psffile        = ../BPTI_ionize.psf # protein structure file
reffile        = ../BPTI_ionize.pdb # PDB file
 
[OUTPUT]
rmsfile        = output.rms       # RMSD file
 
[TRAJECTORY]
trjfile1       = ../BPTI_run.dcd  # trajectory file
md_step1       = 500000           # number of MD steps
mdout_period1  = 500              # MD output period
ana_period1    = 500              # analysis period
repeat1        = 1
trj_format     = DCD              # (PDB/DCD)
trj_type       = COOR+BOX         # (COOR/COOR+BOX)
trj_natom      = 0                # (0:uses reference PDB atom count)
 
[SELECTION]
group1         = an:CA            # selection group 1
 
[FITTING]
fitting_method = TR+ROT           # [NO,TR,TR+ROT,TR+ZROT,XYTR,XYTR+ZROT]
fitting_atom   = 1                # atom group
mass_weight    = NO               # mass-weight is not applied
 
[OPTION]
check_only     = NO               # (YES/NO)
analysis_atom  = 1                # atom group
```

## Ex2: RMSD of the ligand by fitting protein Calpha atoms

```toml
[INPUT]
psffile        = ../protein_atp.psf # protein structure file
reffile        = ../protein_atp.pdb # PDB file
 
[OUTPUT]
rmsfile        = output.rms       # RMSD file
 
[TRAJECTORY]
trjfile1       = ../protein_atp.dcd  # trajectory file
md_step1       = 500000           # number of MD steps
mdout_period1  = 500              # MD output period
ana_period1    = 500              # analysis period
repeat1        = 1
trj_format     = DCD              # (PDB/DCD)
trj_type       = COOR+BOX         # (COOR/COOR+BOX)
trj_natom      = 0                # (0:uses reference PDB atom count)
 
[SELECTION]
group1         = an:CA            # selection group 1
group2         = rnam:ATP
 
[FITTING]
fitting_method = TR+ROT           # [NO,TR,TR+ROT,TR+ZROT,XYTR,XYTR+ZROT]
fitting_atom   = 1                # atom group
mass_weight    = NO               # mass-weight is not applied
 
[OPTION]
check_only     = NO               # (YES/NO)
analysis_atom  = 2                # atom group
```
