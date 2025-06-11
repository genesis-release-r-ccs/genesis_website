---
title: "GENESIS Input Example: Pressure REMD simulation in the NPT ensemble"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---

In the Pressure REMD simulation, pressure of each replica in the NPT ensemble is
exchanged. In the following sample, we use Langevin thermostat and barostat with
the leapfrog integrator. We can also use Berendsen and Bussi thermostat and
barostat with the velocity Verlet integrator (For the combination rule, please
see [this page](/examples/md_simulation_in_the_npt_ensemble_with_langevin__berendsen__and_bussi_thermostat_and_barostat/)).
In addition, if we use the NPAT or NPγT ensemble, normal pressure (\\(P_z\\)) of
the membrane system can be exchanged.

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
type1            = PRESSURE         # Pressure exchange
nreplica1        = 5                # Number of replicas
parameters1      = 1 10 50 100 200  # Pressure (atm)
cyclic_params1   = NO 

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
ensemble         = NPT              # NPT ensemble
tpcontrol        = LANGEVIN         # thermostat/barostat
temperature      = 300.0            # target temperature in each replica (K)
gamma_t          = 1.0              # thermostat friction coefficient (ps-1)
gamma_p          = 0.1              # barostat friction coefficient (ps-1)
isotropy         = ISO              # isotropic pressure coupling

[BOUNDARY]
type             = PBC              # periodic boundary condition
```
