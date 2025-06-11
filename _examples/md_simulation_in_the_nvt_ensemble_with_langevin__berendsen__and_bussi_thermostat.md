---
title: "GENESIS Input Example: MD simulation in the NVT ensemble with Langevin, Berendsen, and Bussi thermostat"
excerpt: ""
last_modified_at: 2025-06-11T22:49:38+09:00
layout: single
classes: wide
toc: false
toc_sticky: false
sidebar:
  nav: sidebar-basic
---


In GENESIS, three types of thermostat are available: Langevin, Berendsen, and
Bussi (Stochastic velocity rescaling) methods. Available combinations between
thermostats and integrators are as follows:

| Thermostat | Integrator |
|:---:|:---:|
|  LANGEVIN |  LEAP, VVER |
| BERENDSEN |  LEAP, VVER |
|   BUSSI   |     VVER |

Thermostat is specified by `tpcontrol` in the `[ENSEMBLE]` section, and
integrator is specified by `integrator` in the `[DYNAMICS]` section. To control
the temperature, `gamma_t` is used in `LANGEVIN`, and `tau_t` is used in
`BERENDSEN` and `BUSSI`.

## Langevin thermostat

```toml
[INPUT]
topfile  = top_all36_prot.rtf     # topology file
parfile  = par_all36_prot.prm     # parameter file
strfile  = toppar_water_ions.str  # stream file
psffile  = ionized.psf            # protein structure file
pdbfile  = ionized.pdb            # input PDB file
rstfile  = minimized.rst          # restart file
 
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
integrator       = LEAP      # [LEAP,VVER]
nsteps           = 10000     # number of MD steps
timestep         = 0.002     # timestep (ps)
eneout_period    =   100     # energy output period
crdout_period    =   100     # coordinates output period
rstout_period    = 10000     # restart output period
nbupdate_period  =    10     # nonbond update period

[CONSTRAINTS]
rigid_bond       = YES       # constraints all bonds involving hydrogen

[ENSEMBLE]
ensemble         = NVT       # NVT ensemble
tpcontrol        = LANGEVIN  # thermostat
temperature      = 300.0     # target temperature (K)
gamma_t          = 1.0       # thermostat friction coefficient (ps-1)

[BOUNDARY]
type             = PBC       # periodic boundary condition
```

## Berendsen thermostat

```toml
[DYNAMICS]
integrator       = LEAP      # [LEAP,VVER]
nsteps           = 10000     # number of MD steps
timestep         = 0.002     # timestep (ps)
eneout_period    = 100       # energy output period
crdout_period    = 100       # coordinates output period
rstout_period    = 10000     # restart output period
nbupdate_period  = 10        # nonbond update period

[ENSEMBLE]
ensemble         = NVT       # NVT ensemble
tpcontrol        = BERENDSEN # thermostat
temperature      = 300.0     # target temperature (K)
tau_t            = 5.0       # temperature coupling time (ps)
```

## Bussi thermostat (Stochastic velocity rescaling method)

```toml
[DYNAMICS]
integrator       = VVER      # [VVER]
nsteps           = 10000     # number of MD steps
timestep         = 0.002     # timestep (ps)
eneout_period    = 100       # energy output period
crdout_period    = 100       # coordinates output period
rstout_period    = 10000     # restart output period
nbupdate_period  = 10        # nonbond update period

[ENSEMBLE]
ensemble         = NVT       # NVT ensemble
tpcontrol        = BUSSI     # thermostat
temperature      = 300.0     # target temperature (K)
tau_t            = 5.0       # temperature coupling time (ps)
```
