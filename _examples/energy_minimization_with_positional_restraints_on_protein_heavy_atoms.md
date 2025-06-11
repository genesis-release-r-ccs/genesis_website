---
title: "GENESIS Input Example: Energy minimization with positional restraints on protein heavy atoms"
excerpt: ""
last_modified_at: 2025-06-11T22:49:37+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---

In the initial structure, there may be artificial atomic crash in some parts of
the system, which causes unstable energy minimization. The
keywords `contact_check = YES` and `nonb_limiter = YES` allow us to avoid such
situation. These options can reduce large forces on the crashing atoms. For
details, please see the user manual (`[ENERGY]` section).


```toml
[INPUT]
topfile  = top_all36_prot.rtf, top_all36_lipid.rtf  # topology file
parfile  = par_all36_prot.prm, par_all36_lipid.prm  # parameter file
strfile  = toppar_water_ions.str  # stream file
psffile  = ionized.psf            # protein structure file
pdbfile  = ionized.pdb            # initial structure
reffile  = ionized.pdb            # reference coordinates for restraints
 
[OUTPUT]
dcdfile  = min.dcd          # DCD trajectory file
rstfile  = min.rst          # restart file
 
[ENERGY]
forcefield       = CHARMM   # CHARMM force field
electrostatic    = PME      # Particle Mesh Ewald method
switchdist       = 10.0     # switch distance
cutoffdist       = 12.0     # cutoff distance
pairlistdist     = 13.5     # pair-list distance
vdw_force_switch = YES      # force switch option for van der Waals
pme_nspline      = 4        # order of B-spline in [PME]
pme_max_spacing  = 1.0      # max grid spacing 
contact_check    = YES      # check atomic crash
nonb_limiter     = YES      # failure caused by atomic crash is avoided

[MINIMIZE]
method           = SD       # Steepest descent method
nsteps           = 5000     # number of minimization steps
eneout_period    =  100     # energy output period
crdout_period    =  100     # coordinates output period
rstout_period    = 5000     # restart output period
nbupdate_period  =   10     # nonbond update period
 
[BOUNDARY]
type             = PBC      # periodic boundary condition
box_size_x       = 128.000  # box size (x) in [PBC]
box_size_y       = 128.000  # box size (y) in [PBC]
box_size_z       = 144.000  # box size (z) in [PBC]

[SELECTION]
group1           = sid:PROA and heavy
 
[RESTRAINTS]
nfunctions       = 1        # number of functions
function1        = POSI     # restraint function type
direction1       = ALL      # direction
constant1        = 1.0      # force constant
select_index1    = 1        # restrained groups
```
