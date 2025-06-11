---
title: "GENESIS Input Example: Diffusion constant (diffusion_analysis)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:37+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


`diffusion_analysis` calculates the translational diffusion coefficients from
either simulation trajectory files or MSD data files produced by `msd_analysis`.
It is necessary to understand how msd_analysis works before using
diffusion_analysis, so please study the
[msd_analysis](/examples/mean-square_displacement_msd_analysis/) page if you
haven't done so already.

The following text is the help message that is output with `./diffusion_analysis
-h ctrl`:

```
# This program calculates translational diffusion coefficient.

# The translational diffusion coefficient is
# computed from MSD data by a simple least
# squares fitting method.

# This program uses MSD output produced by
# msd_analysis. The "rmsfile" file in the
# [OUTPUT] section specifies this file. If the
# file does not exist, depending on the
# "msd_mode", msd_output can be generated
# automatically. In such cases, settings
# in the [INPUT], [OUTPUT], [TRAJECTORY]
# [SELECTION], [MOLECULE_SELECTION] and 
# [MSD_OPTION] are used. Please see msd_analysis
# help for a more detailed description.
# Only settings in [OUTPUT] and [OPTION] are
# used when calculating the diffusion coefficient,
# therefore other sections can be omitted when the
# "rmsfile" file is present.

# If "txtfile" is specified in the [OUTPUT]
# section, the program will output a data file
# containing at least three columns: time in ps,
# MSD in angstroms divided by 6 and points
# produced by the fitted function. The latter
# two columns will be repeated for every MSD data
# set in the "rmsfile".

# The "ndofs" option in [OPTION] section specifies
# the degrees of freedom for the MSD data.
# When the value is a single number, the specified
# degrees of freedom are applied for all MSD sets.
# When the value is a space separated number
# sequence, the degrees of freedom are set
# separately for each MSD data set, but it
# has to match the amount of MSD data sets
# exactly.

[INPUT]
pdbfile = input.pdb    # PDB file
psffile = input.psf    # protein structure file
prmtopfile = input.top # AMBER parameter topology file
ambcrdfile = input.crd # AMBER coordinate file
 
[OUTPUT]
rmsfile = output.rms # RMSD file
txtfile = output.txt # text file
 
[TRAJECTORY]
# trjfile1 = sample.dcd # trajectory file
# md_step1 = 0          # number of MD steps
# mdout_period1 = 0     # MD output period
# ana_period1 = 1       # analysis period
# repeat1 = 1
trj_format = DCD        # (PDB/DCD)
trj_type = COOR+BOX     # (COOR/COOR+BOX)
trj_natom = 0           # (0:uses reference PDB atom count)
 
[SELECTION]
# group1 = all             # selection group 1
# group2 = molname:protein # selection group 2
# mole_name1 = protein P1:1:TYR P1:5:MET
# mole_name2 = lipid OLEO:PCGL:OLEO
 
[MOLECULE_SELECTION]
# selection1 = 0 # selection group number (0 for all)
# mode1 = ALL    # select when ALL/ANY atoms in selection
#                # SET mode specifies atoms explicitly

[MSD_OPTION]
delta = 10       # span of MSD in steps
oversample = yes # smoother graph, but induces bias
#axes1 = x y z   # axes to use for MSD of the first molecule selection

[OPTION]
# msd_mode = REUSE     # REUSE/RECALC/NOCALC
#                      # REUSE: reuse output if available, othervise calculate
#                      # RECALC: always redo MSD calculation and overwrite output
#                      # NOCALC: never do MSD calculation, use provided output
# time_step = 1.00     # time interval (ps) of one step in MSD data
# distance_unit = 1.00 # distance unit of MSD data files in angstroms
# start = 20 %         # data point from which to start fitting
#                      # n %: start at n percent
#                      # n [ps]: start at n picoseconds
# ndofs = 3            # degrees of freedom (i.e. number of axes used) for MSD
#                      # when a single number, sets for all MSD data sets
#                      # when space seperated numbers, sets for each
```

A sample input file to calculate diffusion coefficients when MSD data is
already provided:
```toml
[OUTPUT]
rmsfile = BPTI.msd # msd datd, this can be either
                   # an input or output file depending on msd_mode
txtfile = BPTI.dif # if set, output fitting into this file

[OPTION]
msd_mode = NOCALC # never do MSD calculation, use provided 'BPTI.msd'
time_step = 2     # time interval of one step in MSD data is 2 ps
start = 20 %      # skip initial 19 % of MSD data when fitting
```

A sample input file to calculate diffusion coefficients when MSD data is
not provided. Calculates the MSD for only `x` and `y` axes of the
molecules with residue name `TIP3` (same as sample 2 in [msd_analysis](/examples/mean-square_displacement_msd_analysis/)):
```toml
[INPUT]
psffile = BPTI.psf
pdbfile = BPTI.pdb

[OUTPUT]
rmsfile = BPTI.msd # msd datd, this can be either
                   # an input or output file depending on msd_mode
txtfile = BPTI.dif # if set, output fitting into this file

[TRAJECTORY]
trjfile1 = BPTI.dcd
md_step1 = 10000     # number of MD steps
mdout_period1 = 1000 # MD output period
ana_period1 = 1      # analysis period
trj_format = DCD     # (PDB/DCD)
trj_type = COOR+BOX  # (COOR/COOR+BOX)
trj_natom = 0        # (0:uses reference PDB atom count)

[SELECTION]
group1 = rnam:TIP3 # atoms with residue name 'TIP3' are group 1

[MOLECULE_SELECTION]
selection1 = 1     # use group 1 for molecule selection
mode1 = ALL        # select molecule if all of its atoms are in group 1

[MSD_OPTION]
oversample = true  # smoother graph, but induces bias
delta = 9          # span of MSD in steps
axes1 = z x        # axes to use for MSD of the first molecule selection

[OPTION]
msd_mode = RECALC # always redo MSD calculation and overwrite 'BPTI.msd'
time_step = 2     # time interval of one step in MSD data is 2 ps
start = 20 %      # skip initial 19 % of MSD data when fitting
ndofs = 2         # number of degrees of freedom is 2
```
