---
title: "GENESIS Input Example: MD simulation in the NVE ensemble with the CHARMM, AMBER, and coarse-grained force fields"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


If you use different force fields, only `[INPUT]` and `[ENERGY]` sections are
differently specified, and other sections are basically common. In this page, we
show sample control files for CHARMM, AMBER, and coarse-grained force fields.

## CHARMM force field

In order to use the CHARMM force fields, we need to
specify `topfile` and `parfile` (and `strfile` if needed) in the `[INPUT]`
section. Topology and parameter files are available from the [MacKerell's
homepage](http://mackerell.umaryland.edu/charmm_ff.shtml). To prepare input
`psffile` and `pdbfile`, we usually utilize psfgen, VMD, or CHARMM. In the
`[ENERGY]` section, `forcefield = CHARMM` is used, and `vdw_force_switch = YES`
is recommended for the CHARMM C36 force field.


```toml
[INPUT]
topfile  = top_all36_prot.rtf, top_all36_lipid.rtf  # topology file
parfile  = par_all36_prot.prm, par_all36_lipid.prm  # parameter file
strfile  = toppar_water_ions.str  # stream file
psffile  = ionized.psf            # protein structure file
pdbfile  = ionized.pdb            # input PDB file
rstfile  = minimized.rst          # GENESIS restart file
 
[OUTPUT]
dcdfile  = md.dcd            # DCD trajectory file
rstfile  = md.rst            # restart file

[ENERGY]
forcefield       = CHARMM    # CHARMM force field
electrostatic    = PME       # Particle Mesh Ewald method
switchdist       = 10.0      # switch distance
cutoffdist       = 12.0      # cutoff distance
pairlistdist     = 13.5      # pair-list distance
vdw_force_switch = YES       # force switch option for van der Waals
pme_nspline      = 4         # order of B-spline in [PME]
pme_max_spacing  = 1.0       # max grid spacing 

[DYNAMICS]
integrator       = LEAP      # Leapfrog Verlet integrator
nsteps           = 10000     # number of MD steps
timestep         = 0.002     # timestep (ps)
eneout_period    =   100     # energy output period
crdout_period    =   100     # coordinates output period
rstout_period    = 10000     # restart output period
nbupdate_period  =    10     # nonbond update period

[CONSTRAINTS]
rigid_bond       = YES       # constraints all bonds involving hydrogen

[ENSEMBLE]
ensemble         = NVE       # NVE ensemble
tpcontrol        = NO        # No thermostat and barostat
temperature      = 300.0     # initial temperature (K)

[BOUNDARY]
type             = PBC       # periodic boundary condition
```

## AMBER force field

In order to use the AMBER force fields, we need to specify `prmtopfile` and
coordinates file (`ambcrdfile` or/and `pdbfile`; if both of them specified,
coordinates in `ambcrdfile` will be ignored). The input files can be created
with LEaP module of [AmberTools](http://ambermd.org/).

- Velocities and periodic box size in `ambcrdfile` are always ignored in GENESIS.
  - The initial box size of the simulation is given by `box_size_x`,` y`, and
  `z` in the `[BOUNDARY]` section, or loaded from `rstfile` in the `[INPUT]`
  section.
- In the `[ENERGY]` section, [`forcefield = AMBER`] must be specified
  - `switchdist` and `cutoffdist` must be the same value.
- Pay special attention on `water_model` in `[CONSTRAINTS]` section, where
`water_model = WAT` should be specified if your system has water molecules named
"WAT".

```toml
[INPUT]
prmtopfile  = input.prmtop         # input prmtop file
ambcrdfile  = input.inpcrd         # amber crd file
pdbfile     = input.pdb            # pdb file
rstfile     = minimized.rst        # GENESIS restart

[OUTPUT]
dcdfile     = md.dcd
rstfile     = md.rst

[ENERGY]
forcefield       = AMBER     # AMBER force field
electrostatic    = PME       # Particle Mesh Ewald method
switchdist       = 8.0       # switch distance
cutoffdist       = 8.0       # cutoff distance
pairlistdist     = 10.0      # pair-list distance
pme_nspline      = 4         # order of B-spline in [PME]
pme_max_spacing  = 1.0       # max grid spacing 

[DYNAMICS]
integrator       = LEAP      # Leapfrog Verlet integrator
nsteps           = 10000     # number of MD steps
timestep         = 0.002     # timestep (ps)
eneout_period    = 100       # energy output period
crdout_period    = 100       # coordinates output period
rstout_period    = 10000     # restart output period
nbupdate_period  = 10        # nonbond update period

[CONSTRAINTS]
rigid_bond       = YES       # constraints all bonds involving hydrogen
water_model      = WAT       # water molecule name

[ENSEMBLE]
ensemble         = NVE       # NVE ensemble
tpcontrol        = NO        # No thermostat and barostat
temperature      = 300.0     # initial temperature (K)

[BOUNDARY]
type             = PBC       # periodic boundary condition

```
