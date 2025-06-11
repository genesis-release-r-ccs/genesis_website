---
title: "GENESIS Input Example: Replica-exchange umbrella sampling (REUS or Hamiltonian REMD) using a distance restraint"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---

REUS simulation can be done by just adding the `[REMD]` section in the control
file of the conventional MD simulation with restraints and modifying the
`[INPUT]`/`[OUTPUT]` sections. The following sample is a REUS simulation using
distance restraints with 6 replicas. During the simulation, force constants and
target distance values are exchanged between replicas. Note that the parameter
rest_function in the `[REMD]` section points out the index of restraint function
defined in the `[RESTRAINTS]` section.

```toml
[INPUT]
topfile  = top_all36_prot.rtf       # topology file
parfile  = par_all36_prot.prm       # parameter file
strfile  = toppar_water_ions.str    # stream file
psffile  = ionized.psf              # protein structure file
pdbfile  = ionized.pdb              # input PDB file
rstfile  = eq_rep{}.rst             # restart file
 
[OUTPUT]
logfile  = run_rep{}.log            # log file
dcdfile  = run_rep{}.dcd            # coordinates trajectory file
rstfile  = run_rep{}.rst            # restart file
remfile  = run_rep{}.rem            # parameter index file

[REMD]
dimension        = 1                # Number of dimension of REMD simulation
exchange_period  = 2500             # attempt exchange every 5 ps
type1            = RESTRAINT        # restraint function is exchanged
nreplica1        = 6                # number of replicas
rest_function1   = 1                # target restraint function index

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
nsteps           = 50000            # number of MD steps
timestep         = 0.002            # timestep (ps)
eneout_period    =   100            # energy output period
crdout_period    =   100            # coordinates output period
rstout_period    = 50000            # restart output period
nbupdate_period  =    10            # nonbond update period

[CONSTRAINTS]
rigid_bond       = YES              # constraints all bonds involving hydrogen

[ENSEMBLE]
ensemble         = NVT              # NVT ensemble
tpcontrol        = LANGEVIN         # thermostat
temperature      = 300.0            # target temperature (K)
gamma_t          = 1.0              # thermostat friction coefficient (ps-1)

[BOUNDARY]
type             = PBC              # periodic boundary condition

[SELECTION]
group1           = an:N and resno: 1         # select atom i
group2           = an:O and resno: 10        # select atom j

[RESTRAINTS]
nfunctions       = 1
function1        = DIST                      # use distance restraint
select_index1    = 1 2                       # restrained atom index i and j
constant1        =  10  10  10  10  10  10   # force constant of each replica
reference1       = 4.0 4.3 4.5 4.7 4.9 5.1   # target distance of each replica
```

In this case, we define parameter1 = (10, 4.0), parameter2 = (10, 4.3), …, and
parameter6 = (10, 5.1), and each replica has one of these parameters.