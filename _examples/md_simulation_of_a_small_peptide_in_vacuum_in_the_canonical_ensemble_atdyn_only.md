---
title: "GENESIS Input Example: MD simulation of a small peptide in vacuum in the canonical ensemble (atdyn only)"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---

Because `spdyn` is only applicable to periodic boundary condition (PBC), `atdyn`
is used for a non-periodic boundary condition (NOBC). In the NOBC simulation,
isobaric-isothermal ensemble (NPT) is not available. In addition, the volume (V)
is not considered even if NVE or NVT is specified in the keyword "ensemble".


```toml
[INPUT]
topfile  = top_all36_prot.rtf     # topology file
parfile  = par_all36_prot.prm     # parameter file
psffile  = peptide.psf            # protein structure file
pdbfile  = peptide.pdb            # input PDB file
 
[OUTPUT]
dcdfile  = md.dcd            # DCD trajectory file
rstfile  = md.rst            # restart file

[ENERGY]
forcefield       = CHARMM    # CHARMM force field
electrostatic    = CUTOFF    # Particle Mesh Ewald method
switchdist       =  28.0     # switch distance
cutoffdist       =  30.0     # cutoff distance
pairlistdist     =  32.0     # pair-list distance
vdw_force_switch =  YES      # force switch option for van der Waals 

[DYNAMICS]
integrator       = LEAP      # Leapfrog Verlet integrator
nsteps           = 10000     # number of MD steps
timestep         = 0.002     # timestep (ps)
eneout_period    =   100     # energy output period
crdout_period    =   100     # coordinates output period
stoptr_period    =  1000     # remove translation and rotation of COM
rstout_period    = 10000     # restart output period
nbupdate_period  =    10     # nonbond update period

[CONSTRAINTS]
rigid_bond       = YES       # constraints all bonds involving hydrogen

[ENSEMBLE]
ensemble         = NVT       # NVT ensemble
tpcontrol        = LANGEVIN  # thermostat
temperature      = 298.15    # initial and target temperature (K)
gamma_t          = 1.0       # thermostat friction coefficient (ps-1)

[BOUNDARY]
type             = NOBC      # No boundary condition
```
