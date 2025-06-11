---
title: "GENESIS Input Example: Mean-square displacement (`msd_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---

`msd_analysis` calculates the average mean square deviation (MSD) of a group of
molecules from a simulation trajectory. The output format if understood by
`diffusion_analysis`, that can be used to calculate diffusion coefficients.

The following text is the help message that is output with `./msd_analysis -h
ctrl`:


```
# This program calculates mean square displacement.

# The output is composed of at least two columns.
# The first column corresponds to time, while
# the rest are the mean square displacement for
# each selection. The units are the interval and
# the square units of length (usually angstrom),
# at which the trajectory files were generated.
# The user himself has to convert them if needed.

# Molecules for which to calculate the MSD can be
# specified by selection1 and mode1 in the
# [MOLECULE_SECTION], while the default is to
# use all molecules. With "mode1 = ALL", only the
# molecules whose all atoms are in selection1 will
# be selected, while with "mode1 = ANY", molecules
# containing at least one atom in selection1 will
# be selected. Use selection2, molde2, etc. for
# more selections.

# All molecules are unwrapped before calculation.
# The center of mass of the system is also
# calculated using these unwrapped coordinates
# and is subtracted from all molecule positions.

# The default is to read "delta" number of steps
# and calculate the square displacement by
# shifting "delta" by one step at a time.
# This leads to undersampling of the steps
# at the start and the end of the trajectory.
# This can be turned off by "oversample = NO",
# in which case each trajectory sample will be
# used only once

# The axes used for MSD calculation can be
# specified in the [OPTION] section by
# the axes# option, where # is the selection
# number. For example, to set x and z axes for
# the 7th selection use "axes7 = z X". Letter
# case, order and whitespace does not matter,
# but only x, y and z letters are accepted.

# This program allows uses some simple OpenMP
# acceleration. Please set OMP_NUM_THREADS=1
# to disable it.

[INPUT]
pdbfile = input.pdb    # PDB file
psffile = input.psf    # protein structure file
prmtopfile = input.top # AMBER parameter topology file
ambcrdfile = input.crd # AMBER coordinate file
 
[OUTPUT]
rmsfile = output.rms # RMSD file
 
[TRAJECTORY]
# trjfile1 = sample.dcd # trajectory file
# md_step1 = 0          # number of MD steps
# mdout_period1 = 0     # MD output period
# ana_period1 = 1       # analysis period
# repeat1 = 1
trj_format = DCD    # (PDB/DCD)
trj_type = COOR+BOX # (COOR/COOR+BOX)
trj_natom = 0       # (0:uses reference PDB atom count)
 
[SELECTION]
# group1 = all             # selection group 1
# group2 = molname:protein # selection group 2
# mole_name1 = protein P1:1:TYR P1:5:MET
# mole_name2 = lipid OLEO:PCGL:OLEO
 
[MOLECULE_SELECTION]
# selection1 = 0 # selection group number (0 for all)
# mode1 = ALL    # select when ALL/ANY atoms in selection
#                # SET mode specifies atoms explicitly

[OPTION]
delta = 10       # span of MSD in steps
oversample = yes # smoother graph, but induces bias
#axes1 = x y z   # axes to use for MSD of the first molecule selection
```

A sample input file to calculate the MSD of all molecules:
```toml
[INPUT]
psffile        = BPTI.psf # protein structure file
pdbfile        = BPTI.pdb # PDB file

[OUTPUT]
rmsfile        = BPTI.msd # MSD file

[TRAJECTORY]
trjfile1       = BPTI.dcd        # trajectory file
md_step1       = 10000           # number of MD steps
mdout_period1  = 1000            # MD output period
ana_period1    = 1               # analysis period
trj_format     = DCD             # (PDB/DCD)
trj_type       = COOR+BOX        # (COOR/COOR+BOX)
trj_natom      = 0               # (0:uses reference PDB atom count)

[MOLECULE_SELECTION]
selection1 = 0      # select all molecules

[OPTION]
oversample = true   # smoother graph, but induces bias
delta = 9           # span of MSD in steps
```

A sample input file to calculate the MSD for only `x` and `z` axes of
the molecules with residue name `TIP3:`
```toml
[INPUT]
psffile        = BPTI.psf # protein structure file
pdbfile        = BPTI.pdb # PDB file

[OUTPUT]
rmsfile        = BPTI.msd # MSD file

[TRAJECTORY]
trjfile1       = BPTI.dcd        # trajectory file
md_step1       = 10000           # number of MD steps
mdout_period1  = 1000            # MD output period
ana_period1    = 1               # analysis period
trj_format     = DCD             # (PDB/DCD)
trj_type       = COOR+BOX        # (COOR/COOR+BOX)
trj_natom      = 0               # (0:uses reference PDB atom count)

[SELECTION]
group1 = rnam:TIP3  # atoms with residue name 'TIP3' are group 1

[MOLECULE_SELECTION]
selection1 = 1      # use group 1 for molecule selection
mode1 = ALL         # select molecule if all of its atoms are in group 1

[OPTION]
oversample = true   # smoother graph, but induces bias
delta = 9           # span of MSD in steps
axes1 = z x         # axes to use for MSD of the first molecule selection
```

A sample input file to calculate the MSD of a single protein complex with
segment ID `BPTI`. There must be only a single protein complex and all of its
atoms must be specified by selection:
```toml
[INPUT]
psffile        = BPTI.psf # protein structure file
pdbfile        = BPTI.pdb # PDB file

[OUTPUT]
rmsfile        = BPTI.msd # MSD file

[TRAJECTORY]
trjfile1       = BPTI.dcd        # trajectory file
md_step1       = 10000           # number of MD steps
mdout_period1  = 1000            # MD output period
ana_period1    = 1               # analysis period
trj_format     = DCD             # (PDB/DCD)
trj_type       = COOR+BOX        # (COOR/COOR+BOX)
trj_natom      = 0               # (0:uses reference PDB atom count)

[SELECTION]
group1 = segid:BPTI # atoms with segment ID 'BPTI' are group 1

[MOLECULE_SELECTION]
selection1 = 1      # use group 1 for molecule selection
mode1 = SET         # set atoms in grup1 as a single molecule

[OPTION]
oversample = true   # smoother graph, but induces bias
delta = 9           # span of MSD in steps
```

A sample input to do the same MSD computation as the previous three
samples in a single calculation:
```toml
[INPUT]
psffile        = BPTI.psf # protein structure file
pdbfile        = BPTI.pdb # PDB file

[OUTPUT]
rmsfile        = BPTI.msd # MSD file

[TRAJECTORY]
trjfile1       = BPTI.dcd        # trajectory file
md_step1       = 10000           # number of MD steps
mdout_period1  = 1000            # MD output period
ana_period1    = 1               # analysis period
trj_format     = DCD             # (PDB/DCD)
trj_type       = COOR+BOX        # (COOR/COOR+BOX)
trj_natom      = 0               # (0:uses reference PDB atom count)

[SELECTION]
group1 = rnam:TIP3  # atoms with residue name 'TIP3' are group 1
group2 = segid:BPTI # atoms with segment ID 'BPTI' are group 2

[MOLECULE_SELECTION]
selection1 = 0      # select all molecules
selection2 = 1      # use group 1 for molecule selection 2
mode2 = ALL         # select molecule if all of its atoms are in group 2
selection3 = 2      # use group 2 for molecule selection 3
mode3 = SET         # set atoms in group 2  as a single molecule

[OPTION]
oversample = true   # smoother graph, but induces bias
delta = 9           # span of MSD in steps
axes2 = z x         # axes to use for MSD of the second molecule selection
```
