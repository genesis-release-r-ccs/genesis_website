---
title: "GENESIS Input Example: Manipulate trajectories (combining, thinning, fitting, wrapping, centering, format change) (`crd_convert`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


This program can manupulate trajectory data. For example, during the MD
simulations, water molecules spread out from  the unit cell, which can
be wrapped into the unit cell by using `crd_convert`. In addition, the
user can make a new trajectory file, where the focusing atoms are fitted
to the initial structure, centered at the favorite position, and etc.
Note that trajectory manipulation is done in the following order:
1. Centering
2. Wrapping
3. Fitting
To change this order, the users should modify the source codes in
`cc_convert.fpp` in `crd_convert` by themselves.

This program can also convert the DCD format to PDB format. If the user
specify `split_trjpdb = YES`, all snapshots in the trajectory files are split
into individual PDB files.

## Target protein is centered at the origin, and water and ions are wrapped into the unit cell 


```toml
[INPUT]
psffile        = ../BPTI_ionize.psf # protein structure file
reffile        = ../BPTI_ionize.pdb # PDB file

[OUTPUT]
trjfile        = output.dcd      # trajectory file
 
[TRAJECTORY]
trjfile1       = ../BPTI_run.dcd # trajectory file
md_step1       = 500000          # number of MD steps
mdout_period1  = 500             # MD output period
ana_period1    = 500             # analysis period
repeat1        = 1
trj_format     = DCD             # (PDB/DCD)
trj_type       = COOR+BOX        # (COOR/COOR+BOX)
trj_natom      = 0               # (0:uses reference PDB atom count)
 
[SELECTION]
group1         = all             # select all atoms
group2         = segid:BPTI      # select protein atoms
 
[FITTING]
fitting_method = NO              # [NO,TR,TR+ROT,TR+ZROT,XYTR,XYTR+ZROT]
 
[OPTION]
check_only     = NO
trjout_format  = DCD             # write DCD format
trjout_type    = COOR+BOX
trjout_atom    = 1               # output all atoms
split_trjpdb   = NO
centering      = YES             # protein is centered at the origin
centering_atom = 2
center_coord   = 0.0 0.0 0.0
pbc_correct    = MOLECULE        # molecules are wrapped into the unit cell
```

## Make a new DCD file that contain only CA atoms of the protein (no fitting and no centering)


```toml
[SELECTION]
group1         = an:CA and segid:BPTI  # select protein CA atoms
 
[FITTING]
fitting_method = NO              # [NO,TR,TR+ROT,TR+ZROT,XYTR,XYTR+ZROT]
 
[OPTION]
check_only     = NO
trjout_format  = DCD             # write DCD format
trjout_type    = COOR+BOX
trjout_atom    = 1               # output all atoms
split_trjpdb   = NO
centering      = NO              # protein is centered at the origin
pbc_correct    = NO
```

## Make a new DCD file that contain only protein CA atoms with fitting to the initial coordinates


```toml
[SELECTION]
group1         = an:CA and segid:BPTI  # select protein CA atoms
 
[FITTING]
fitting_method = TR+ROT                # [NO,TR,TR+ROT,TR+ZROT,XYTR,XYTR+ZROT]
fitting_atom   = 1 

[OPTION]
check_only     = NO
trjout_format  = DCD             # write DCD format
trjout_type    = COOR+BOX
trjout_atom    = 1               # output all atoms
split_trjpdb   = NO
centering      = NO              # protein is centered at the origin
pbc_correct    = NO
```

## Combine several dcd files into one dcd file


```toml
[INPUT]
psffile        = ../BPTI_ionize.psf # protein structure file
reffile        = ../BPTI_ionize.pdb # PDB file

[OUTPUT]
trjfile        = output.dcd      # trajectory file
 
[TRAJECTORY]
trjfile1       = ../BPTI_run1.dcd # trajectory file
trjfile2       = ../BPTI_run2.dcd # trajectory file
trjfile3       = ../BPTI_run3.dcd # trajectory file
trjfile4       = ../BPTI_run4.dcd # trajectory file
trjfile5       = ../BPTI_run5.dcd # trajectory file
md_step1       = 500000          # number of MD steps
mdout_period1  = 500             # MD output period
ana_period1    = 500             # analysis period
repeat1        = 5
trj_format     = DCD             # (PDB/DCD)
trj_type       = COOR+BOX        # (COOR/COOR+BOX)
trj_natom      = 0               # (0:uses reference PDB atom count)
 
[SELECTION]
group1         = all             # select all atoms
 
[FITTING]
fitting_method = NO              # [NO,TR,TR+ROT,TR+ZROT,XYTR,XYTR+ZROT]
 
[OPTION]
check_only     = NO
trjout_format  = DCD             # write DCD format
trjout_type    = COOR+BOX
trjout_atom    = 1               # output all atoms
```

## Split coordinates trajectory data into individual PDB file


```toml
[INPUT]
psffile        = ../BPTI_ionize.psf # protein structure file
reffile        = ../BPTI_ionize.pdb # PDB file

[OUTPUT]
trjfile        = output{}.pdb      # trajectory file
 
[TRAJECTORY]
trjfile1       = ../BPTI_run.dcd # trajectory file
md_step1       = 500000          # number of MD steps
mdout_period1  = 500             # MD output period
ana_period1    = 500             # analysis period
repeat1        = 1
trj_format     = DCD             # (PDB/DCD)
trj_type       = COOR+BOX        # (COOR/COOR+BOX)
trj_natom      = 0               # (0:uses reference PDB atom count)
 
[SELECTION]
group1         = all             # select all atoms
group2         = segid:BPTI      # select protein atoms
 
[FITTING]
fitting_method = NO              # [NO,TR,TR+ROT,TR+ZROT,XYTR,XYTR+ZROT]
 
[OPTION]
check_only     = NO
trjout_format  = PDB             # write PDB
trjout_type    = COOR+BOX
trjout_atom    = 1               # output all atoms
split_trjpdb   = YES
centering      = YES             # protein is centered at the origin
centering_atom = 2
center_coord   = 0.0 0.0 0.0
pbc_correct    = MOLECULE        # molecules are wrapped into the unit cell
```
