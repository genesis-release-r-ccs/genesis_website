---
title: "GENESIS Input Example: Density (`density_analysis`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:37+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---

This tool calculates a spatial density distribution of 'solvent' atoms around
'solute' atoms from input trajectory files. It is better for density calculation
if the trajectory was simulated in constant volume condition (i.e. NVT or NVE
condition).

It computes electron density distribution (`density_option` = `ELECTRON`) or
simply number density distribution (`density_option` = `NUMBER`). When
`density_option` = `ELECTRON`, this tool computes the average number of
electrons (*q* = the atomic number -- the partial charge) of 'solvent' atoms
visiting each voxel:

\\[ \rho_e(x, y, z) = \sum_i q(x, y, z) / N_{snapshot} \\] 
\\(i\\): snapshot number, \\(N_{snapshot}\\): total number of snapshots used in
the calculation, if any solvent atom does not visit a voxel at \\((x, y, z)\\),
\\(q(x, y, z) = 0.0\\) in the snapshot.



The atomic number of each atom is calculated from atom_type written in a
psf/prmtop file. You can check these values used in analyses by viewing a log
file when you set `verbose` = `YES`.  When `density_option` = `number`, the tool
computes simply the average of the number of times that 'solvent' atoms visited
in each voxel:

\\[ \rho_N(x, y, z) = \sum_i \\delta(x, y, z) / N_{snapshot} \\] 
\\(\\delta\\): if a solvent atom visits the voxel in a snapshot, \\(\delta =1\\); otherwise \\(\delta = 0\\).

The calculated spatial density distribution is output in any of the three
formats: [CCP4](http://structure.usc.edu/ccp4/maplib.html),
[XPLOR/CNS](http://www.msg.ucsf.edu/local/programs/cns1.3/tutorial/formats/maps/text.html)
and OpenDX. Molecular visualization programs like [PyMOL](https://pymol.org/2/)
,[VMD](http://www.ks.uiuc.edu/Research/vmd/) support these file formats.
[CCP4mg](https://www.ccp4.ac.uk/MG/) program also supports ccp4-format density
file whose extension is ".map". The output file format is specified by
`output_format` in `[DENSITY_OPTION]`. The resolution of the output density
distribution is specified by '`voxel_size`' (the unit is Å).

The box dimensions used for the density distribution calculation are the box
dimensions of the simulation box when `determine_box = TRAJECTORY`. When
`determine_box = MANUAL`, the box dimensions used for the distribution
calculation is equal to the values of `box_size_x, y, z` in `[BOUNDARY]`
section. In this case, the center of the box for the density output is the
center of the mass of the 'solute' molecule. In Both cases, the cutoff (the
thickness of the buffer region) length \\(\Delta r\\) should be larger than the
range used for the distribution calculation \\(d\\).

Finally, when a calculated density distribution is output, the box dimensions
are shrank to include only voxels where density values are non-zero.

This tool has functions to center a target molecule of interest (`recenter`), to
wrap solvent  molecules (`wrap`) and to superimpose the molecule to a reference
molecule (`fitting_method`). Thus you do not need to run any programs such as
`crd_convert` before you use this tool.


```toml
[INPUT]
psffile = BPTI_solvate.psf
reffile = BPTI_solvate.pdb
pdbfile = BPTI_solvate.pdb

[OUTPUT]
mapfile = BPTI_hydration.ccp4  # file name for the 3D-distribution output
pdbfile = BPTI_density.pdb     # please use this file when you view the 3D density together with the reference structure.

[TRAJECTORY]
trjfile1      = run.dcd
md_step1      = 1000
mdout_period1 = 1
ana_period1   = 1

trj_format    = DCD
trj_type      = COOR+BOX

[BOUNDARY]
type          = PBC
domain_x      = 2
domain_y      = 2
domain_z      = 2
num_cells_x   = 70
num_cells_y   = 70
num_cells_z   = 70

[ENSEMBLE]
ensemble      = NVE   # NVT or NVE ensemble is recommended 

[SELECTION]
group1        = resno:1-58 & an:CA
group2        = resname:TIP3
group3        = resno:1-58

[FITTING]
fitting_method = TR+ROT
fitting_atom   = 1
mass_weight    = NO

[SPANA_OPTION]
wrap     = yes
buffer   = 10.0
box_size = TRAJECTORY       # (TRAJECTORY / MANUAL / MAX)

[DENSITY_OPTION]
density_type   = ELECTRON   # (ELECTRON / NUMBER)
output_format  = CCP4       # (CCP4 / XPLOR / DX)
verbose        = no         # if yes, 
                            # partial charge and atomic number of each atom is output in log file
solute         = 3
solvent        = 2
recenter       = 1
range          = 8.0        # calculation range around solute atom
voxel_size     = 1.0        # resolution of the output density
# magnification  = 6.0        # useful when you view the output density distribution using PyMOL software
```
