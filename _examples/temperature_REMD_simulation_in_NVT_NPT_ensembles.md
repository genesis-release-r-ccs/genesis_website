---
title: "GENESIS Input Example: Temperature REMD simulation in the NVT and NPT ensembles"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


## Temperature REMD simulation in the NVT ensembles

Basically, REMD simulation can be done by just adding the `[REMD]` section in the
control file of the conventional MD simulation and modifying the `[INPUT]` and
`[OUTPUT]` sections. Here, we show a sample control file for Temperature REMD
simulation in the NVT ensemble. NPT, NPAT, and NPγT ensembles can be also
employed by modifying the `[ENSEMBLE]` section. In those cases, the pressure and
surface tension in each replica are specified by the keywords pressure and gamma
in the `[ENSEMBLE]` section.

The user may first prepares input rstfiles (`eq_rep1.rst`, `eq_rep2.rst`, …,
`eq_rep8.rst`) that were obtained from equilibration run at each temperature
(300.00, 304.54, …, 332.99K). After executing the following control file, the
user obtains `run_rep1.log`, `run_rep1.dcd`, `run_rep1.rst`, `run_rep1.rem` from
Replica 1, `and run_rep2.log`, `run_rep2.dcd`, `run_rep2.rst`, `run_rep2.rem`
from Replica 2.  Note that replica index (NOT parameter index) is automatically
inserted into the bracket “`{}`” of the input and output filename. In order to
sort coordinates by temperature, remd_convert in the GENESIS analysis tool set
is used.


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
type1            = TEMPERATURE      # Temperature exchange
nreplica1        = 8                # Number of replicas
parameters1      = 300.00 304.54 309.14 313.80 318.51 323.28 328.10 332.99
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
ensemble         = NVT              # NVT ensemble
tpcontrol        = LANGEVIN         # thermostat
gamma_t          = 1.0              # thermostat friction coefficient (ps-1)

[BOUNDARY]
type             = PBC              # periodic boundary condition
```


## Temperature REMD simulation in the NPT ensemble

```toml
[REMD]
dimension        = 1                # Number of dimension of REMD simulation
exchange_period  = 2500             # attempt exchange every 5 ps
type1            = TEMPERATURE      # Temperature exchange
nreplica1        = 8                # Number of replicas
parameters1      = 300.00 304.54 309.14 313.80 318.51 323.28 328.10 332.99
cyclic_params1   = NO 

[ENSEMBLE]
ensemble         = NPT              # NPT ensemble
tpcontrol        = LANGEVIN         # thermostat and barostat
pressure         = 1.0              # pressure (atm)
gamma_t          = 1.0              # thermostat friction coefficient (ps-1)
gamma_p          = 0.1              # barostat friction coefficient (ps-1)
```

In the REMD simulation with these control files, replica exchange is attempted
between the following temperature pairs at the interval of 2500 steps, where
neighboring temperature pairs are switched alternatively. If “`cyclic_params1 =
YES`” is used, 300.00K and 332.99K are defined as the neighboring temperatures
(but there should be a very low exchange probability). Detailed information
about parameter exchange is output in the log file.

![](/assets/images/2025_06_remd_pair.png)

In the control file, bracket can be also used in pdbfile in the `[INPUT]` section.

```toml
[INPUT]
topfile  = top_all36_prot_lipid.rtf # topology file
parfile  = par_all36_prot_lipid.prm # parameter file
psffile  = ionized.psf              # protein structure file
pdbfile  = eq_rep{}.pdb             # input PDB file
```

The user can specify the single coordinates file as an input. In this case,
coordinates of all replicas are started from the same structure.

### Case 1: Initial structure is given by pdbfile

```toml
[INPUT]
topfile  = top_all36_prot_lipid.rtf # topology file
parfile  = par_all36_prot_lipid.prm # parameter file
psffile  = ionized.psf              # protein structure file
pdbfile  = eq.pdb                   # input PDB file
```

### Case 2: Initial structure is given by rstfile

```toml
[INPUT]
topfile  = top_all36_prot_lipid.rtf # topology file
parfile  = par_all36_prot_lipid.prm # parameter file
psffile  = ionized.psf              # protein structure file
pdbfile  = ionized.pdb              # input PDB file
rstfile  = eq.rst                   # restart file
```
