---
title: "GENESIS Input Example: Surface-tension REMD simulation for a membrane system"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


Note that the NPγT ensemble is used in surface-tension REMD, where the
temperature and normal pressure in each replica is specified in the `[ENSEMBLE]`
section.

```toml
[INPUT]
topfile  = top_all36_prot.rtf, top_all36_lipid.rtf  # topology file
parfile  = par_all36_prot.prm, par_all36_lipid.prm  # parameter file
strfile  = toppar_water_ions.str                    # stream file
psffile  = ionized.psf                              # protein structure file
pdbfile  = ionized.pdb                              # input PDB file
rstfile  = eq_rep{}.rst                             # restart file
 
[OUTPUT]
logfile  = run_rep{}.log            # log file
dcdfile  = run_rep{}.dcd            # coordinates trajectory file
rstfile  = run_rep{}.rst            # restart file
remfile  = run_rep{}.rem            # parameter index file

[REMD]
dimension        = 1                # Number of dimension of REMD simulation
exchange_period  = 5000             # attempt exchange every 10 ps
type1            = GAMMA            # Surface tension exchange
nreplica1        = 5                # Number of replicas
parameters1      = -10.0 -5.0 0.0 5.0 10.0 # target surface tension (dyn/cm)

[ENERGY]
forcefield       = CHARMM           # CHARMM force field
electrostatic    = PME              # Particle Mesh Ewald method
switchdist       = 10.0             # switch distance
cutoffdist       = 12.0             # cutoff distance
pairlistdist     = 13.5             # pair-list distance
vdw_force_switch = YES              # force switch option for van der Waals
pme_nspline      = 4                # order of B-spline in [PME]
pme_max_spacing  = 1.0              # max grid spacing

[DYNAMICS]
integrator       = LEAP             # Leapfrog Verlet integrator
nsteps           = 500000           # number of MD steps
timestep         =  0.002           # timestep (ps)
eneout_period    =    100           # energy output period
crdout_period    =    100           # coordinates output period
rstout_period    = 500000           # restart output period
nbupdate_period  =     10           # nonbond update period

[CONSTRAINTS]
rigid_bond       = YES              # constraints all bonds involving hydrogen

[ENSEMBLE]
ensemble         = NPgT             # NPgammaT ensemble
tpcontrol        = LANGEVIN         # thermostat
temperature      = 323.15           # target temperature
pressure         = 1.0              # target normal pressure
isotropy         = SEMI-ISO         # semi-isotropic pressure coupling
gamma_t          = 1.0              # thermostat friction coefficient (ps-1)
gamma_p          = 0.1              # barostat friction coefficient (ps-1)

[BOUNDARY]
type             = PBC              # periodic boundary condition
```
