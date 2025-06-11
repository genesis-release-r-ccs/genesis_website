---
title: "GENESIS Input Example: Distance, Angle, Dihedral angle, COM distance, COM angle, COM dihedral angle (`trj_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:37+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---

The followings are the example of the distance calculation.  Basically, same
manners can be used in the case of angle and dihedral angle calculation.

## Calculate the distance between Calpha atoms in Residues 1 and 58

To calculate the distance between two atoms, we specify `disfile` in `[OUTPUT]`,
and select atom pairs by the parameters distance in `[OPTION]`. You can specify
multiple distance pairs by `distance1`, `distance2`, .... In this care, the
output file contains results in multiple columns.

```toml
[INPUT]
psffile       = ../BPTI_ionize.psf  # protein structure file
reffile       = ../BPTI_ionize.pdb  # PDB file
 
[OUTPUT]
disfile       = output.dis          # distance file
 
[TRAJECTORY]
trjfile1      = ../BPTI_run.dcd     # trajectory file
md_step1      = 500000              # number of MD steps
mdout_period1 = 500                 # MD output period
ana_period1   = 500                 # analysis period
repeat1       = 1
trj_format    = DCD                 # (PDB/DCD)
trj_type      = COOR+BOX            # (COOR/COOR+BOX)
trj_natom     = 0                   # (0:uses reference PDB atom count)

[OPTION]
check_only    = NO                  # (YES/NO)
distance1     = BPTI:1:ARG:CA  BPTI:58:ALA:CA
```

If you do not like the above format in the atom selection, the following control
file can be used, where the two atoms are selected by `group1` and `group2` in
`[SELECTION]`, and the center of mass distance is calculated by specifying
`com_distance` in `[OPTION]` and `comdisfile` in `[OUTPUT]`.

```toml
[INPUT]
psffile       = ../BPTI_ionize.psf  # protein structure file
reffile       = ../BPTI_ionize.pdb  # PDB file

[OUTPUT]
comdisfile    = output.comdis       # COM distance file

[TRAJECTORY]
trjfile1      = ../BPTI_run.dcd     # trajectory file
md_step1      = 500000              # number of MD steps
mdout_period1 = 500                 # MD output period
ana_period1   = 500                 # analysis period
repeat1       = 1
trj_format    = DCD                 # (PDB/DCD)
trj_type      = COOR+BOX            # (COOR/COOR+BOX)
trj_natom     = 0                   # (0:uses reference PDB atom count)

[SELECTION]
group1        = an:CA and rno:1     # select single atom
group2        = an:CA and rno:58    # select single atom

[OPTION]
check_only    = NO                  # (YES/NO)
com_distance1 = 1 2
```

## Calculate the center of mass distance between Residues 1 and 58

```toml
[INPUT]
psffile       = ../BPTI_ionize.psf  # protein structure file
reffile       = ../BPTI_ionize.pdb  # PDB file
 
[OUTPUT]
comdisfile    = output.comdis       # COM distance file
 
[TRAJECTORY]
trjfile1      = ../BPTI_run.dcd     # trajectory file
md_step1      = 500000              # number of MD steps
mdout_period1 = 500                 # MD output period
ana_period1   = 500                 # analysis period
repeat1       = 1
trj_format    = DCD                 # (PDB/DCD)
trj_type      = COOR+BOX            # (COOR/COOR+BOX)
trj_natom     = 0                   # (0:uses reference PDB atom count)

[SELECTION]
group1        = rno:1               # COM group 1
group2        = rno:58              # COM group 2

[OPTION]
check_only    = NO                  # (YES/NO)
com_distance1 = 1 2
```
