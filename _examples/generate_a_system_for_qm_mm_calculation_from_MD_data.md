---
title: "GENESIS Input Example: Generate a system for QM/MM calculation from MD data (`qmmm_generator`)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---

Since the majority of MD simulations is performed with the periodic boundary
conditions (PBC), it is necessary to convert the PBC system to a cluster system
for QM/MM calcluations. This utility generates snapshot structures for QM/MM
calculations from MD data. Given the trajectory data (dcd, rst, pdb), the
program carries out the following,

- Center the origin to the QM region (or any other specified region).
- Wrap all molecules to a simulation box.
- Remove unnecessary molecules if any.

and yields the PSF and coordinate files (in pdb and crd) for selected snapshots.


## Generate a cluster system, where a protein is set to the center and solvent molecules are included around 20 Å of the protein

```toml
[INPUT]
psffile      = ../prot_solv.psf  # protein structure file
reffile      = ../prot_solv.pdb  # PDB file

[OUTPUT]
qmmm_crdfile = snapshot{}.crd  # CHARMM CARD file for QMMM calc.
qmmm_psffile = snapshot{}.psf  # CHARMM PSF file for QMMM calc.
qmmm_pdbfile = snapshot{}.pdb  # PDB file for reference of analysis

[TRAJECTORY]
trjfile1 = ../1_equil/nvt.dcd  # MD trajectory file
md_step1      = 5000           # number of MD steps
mdout_period1 = 100            # MD output period
ana_period1   = 100            # analysis period
trj_type      = COOR+BOX       # (COOR/COOR+BOX)
trj_natom     = 0              # (0:uses reference PDB atom count)

[SELECTION]
group1 = segid:PROA                     # Select protein
group2 = segid:PROA or \                # Select protein and molecules 
         (segid:PROA around_mol:20.0)   # 20 Å around the protein.
group3 = segid:PROA and an:CA           # Select Ca of the protein

[FITTING]
fitting_method = TR+ROT   # NO/TR+ROT/TR/TR+ZROT/XYTR/XYTR+ZRO
fitting_atom   = 3        # atom group used for fitting
mass_weight    = NO       # mass-weight is not applied

[OPTION]
check_only          = NO        # only checking input files (YES/NO)
coord_format        = CHARMM    # (CHARMM CARD)
qmmm_atom_index     = 2         # atom groups for QM/MM systems
frame_number        = 10:10:50  # begin:stride:end
reconcile_num_atoms = NO        # (YES/NO)
origin_atom_index   = 1         # atom groups for the origin of the system
```

NOTES:
- The snapshot structures are fit to a reference structure, reffile in `[INPUT]`.
- `frame_number` in `[OPTION]` can be specified in three ways:
  1. Directly input frame numbers with comma or space, for example, 800, 1600, 2400.
  2. Input frame numbers in format of begin:stride:end, for example, 800:800:2400.
  3. The two expressions can be specified simultaneously separating with slash, for example, 1, 5, 9 / 13:4:30.
- The number of solvent molecules (20 Å around the protein) will be different among different frames. If `reconcile_num_atoms` in `[OPTION]` is set to YES, the number of atoms are made as much similar as possible.
- `origin_atom_index` in `[OPTION]` defines the origin of the system. When multiple atoms are specified, the center of mass of these atoms is set to the origin.

